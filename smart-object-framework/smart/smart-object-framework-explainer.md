# Smart Object Framework Explainer

### The Smart Object Framework

The **Smart Object Framework** (SOF) is an abstraction layer built on top of the MUD framework for representing and managing on-chain Entities and different relationships between those entities. The primary goal of the SOF is to ensure compatible and composable Objects and consistent Interfacing for ANY development that intends to integrate with (or extend upon) the EVE Frontier game world. Beyond entity management, the SOF also provides system scoping and execution context tracking while maintaining security and clear permission boundaries through access control management.

### Core Components

#### Execution Context

* Tracks system ResourceId, function selector, msg.sender, and msg.value information for each World system call
* Maintains call stack history and integrity for the lifetime of a transaction
* Provides context validation at each call in the execution
* Uses transient storage to do so

#### Entity Management

* Entities are tagged with either Class or Object property tags
* Objects instantiate Classes and inherit system associations via an Entity Relation Tag which defines an Object's parent classId
* Resource relation Tags create explicit access relationships between systems, Classes and Objects

#### Entity Resource Scoping

* Enforces System-to-Entity relationships
* Validates Entity centric execution boundaries

#### Entity Based Access Control

* Configure and enforce Class or Object level access control
* Provide robust and flexible environment for full access case coverage

### SmartObjectFramework.sol

Much of this behaviour can be used as `modifiers` or `internal function calls` by inheriting from the `SmartObjectFramework` System. All SOF integrated contracts should inherit from the `SmartObjectFramework` contract, since the SmartObjectFramework is where we extend the basic MUD framework with SOF quality of life features. The following are brief descriptions of the `SmartObjectFramework`'s exposed functionality:

#### Execution Context

The Execution Context system extends MUD's `World.sol` and `System.sol` contracts with robust call context tracking and validation. It provides a way to securely track and verify the original caller, value, and system information throughout the execution stack.

Key Additions Over the Base MUD Implementation:

* Extended Call Context Tracking
* Maintains context tracking for the full execution call stack
* Allows accessing context values at any call depth
* Provides total call count tracking
* Alternate Context Validation
* Ensures the World entry point interface is used for all non-root system calls
* Uses alternate transient storage pattern (rather than calldata) for context data tracking and enforcement

**Context Tracking**

Within the World's transient storage a call stack of ordered world calls is built for the entire transaction execution.

To access the call stack depth, there is an exposed function on the World contract for fetching the current call stack depth count:

{% code title="World.sol" %}
```solidity
function getWorldCallCount() public view returns (uint256)
```
{% endcode %}

For each World call in the call stack the following context values are tracked:

* System ResourceId of the current system being called
* Function Selector of the target function to be executed
* call msg.sender
* call msg.value

These values can be retrieved for the current call by calling the following getter on the World contract:

{% code title="World.sol" %}
```solidity
function getWorldCallContext() public view returns (ResourceId, bytes4, address, uint256)
```
{% endcode %}

Or can be fetched for previous calls in the call stack by using:

{% code title="World.sol" %}
```solidity
function getWorldCallContext(uint256 callCount) public view returns (ResourceId, bytes4, address, uint256)
```
{% endcode %}

NOTE: calling the above with a `callCount` that exceed the current `getWorldCallCount()` value will result in producing `null` return values.

**Context Validation**

The context modifier ensures the following:

* The current non-root system (and targeted function) is being called via one of the basic `World.sol` exposed entry points (`world.call()`, `world.callFrom()`, or `world.fallback()`)
* Call context values will be tracked in transient storage for the current call (as long as it is a state changing execution, i.e., a non-view/pure world call)

**Usage**

Example `SmartObjectFramework` system integration with the context modifier and Execution Context getter usage:

{% code title="MySystem.sol" %}
```solidity
import { IWorldWithContext } from "@eveworld/smart-object-framework/src/IWorldWithContext.sol";
import { SmartObjectFramework } from "@eveworld/smart-object-framework/src/inherit/SmartObjectframework.sol";

contract MySystem is SmartObjectFramework {

    IWorldWithContext world = IWorldWithContext.(_world());

    function myFunction() public context {
        // Access current call msg.sender
        address sender = _callMsgSender();

        // Access current call msg.value
        uint256 value = _callMsgValue();

        // Get full current call context
        (ResourceId systemId, bytes4 selector, address sender, uint256 value) = world.getWorldCallContext();

        // Get current call stack depth
        uint256 currentCallCount = world.getWorldCallCount();

        uint256 selectedCall = 1; // 1 is the intial entry call

        // Access select previous call msg.sender
        address selectedCallSender = _callMsgSender(1);

        // Access select previous call msg.value
        uint256 selectedCallValue = _callMsgValue(1);

        // Get previous full call context
        (ResourceId selectedCallSystemId, bytes4 selectedCallSelector, selectedCallSender, selectedCallValue) = world.getWorldCallContext(1);
    }
}
```
{% endcode %}

#### Access Rule Enactment & Configuration

The Access Configuration system provides granular access control configuration through `AccessConfigSystem.sol`. It enables MUD System namespace owners to define and enforce access control logic for any World registered MUD System and function.

Access configuration is only triggered and enforced if the target (the System/function being accessed) implements the `access()` modifier.

If the access modifier is included in a function's implementation, then the SmartObjectFramework will check to see if there is any access logic configured for that target and if the access enforcement toggle is switched on.

Only when the above three conditions are satisfied will the access logic for a function call be triggered.

**Key Features**

* Dynamic Access Control
* Namespace-owner controlled configuration - System namespace-owners are the only ones allowed to configure access rules for each System
* Configure access logic per system function - provides fine grained access control for each call path in a World
* Enable/disable access enforcement at runtime - live access enforcement toggling (turn access on/off at any time)
* Flexible System-level access management - the ability to map any access system function to any target system function

**Access Logic Flexibility**

Support for complex access rule patterns with the following available data:

* Entity level access rule execution - by passing the entityId (Class or Object) to the access modifier, builders can implement access rules for a specific Class of objects, or individual Objects themselves without having to deploy new sets of contracts
* Execution Context values - a builder can use the execution context call stack values to decide exactly which callers and which call paths are allowed for a target. The following is available for any call depth:
  * systemId,
  * functionId,
  * msg.sender
  * msg.value
* Function Parameters - the target function parameter `calldata` is available, allowing for custom usage in access rule building (after `abi.decode`ing the parameter values)

With the above available data, complex and bespoke access rules can be built for nearly any consideration.

NOTE: as logic patterns emerge in the development space, builders can easily point to other pre-existing access logic deployments for their own configurations, minimizing the costs of access logic management when possible.

**How Access Configuration Works**

Implementing the access modifier on the target function:

* the access modifier accepts an `entityId` (can be a `classId`, `objectId`, or `uint256(0)`)
* `classId` to be used for Class level access rules. e.g., when a Class "owner" (or similarly approved account) should be checked for access, or when a specific set of System addresses are allowed for Class level access (beyond the normal scope rules)
* `objectId` to be used for Object level access rules. e.g., when an Object "owner" (or similarly approved account) should be checked for access, or Object specific System access. Or in cases where access needs to be checked against the specific Object's parent Class.
* `uint256(0)` to be used when Class/Object considerations are not relevant. This kind of configuration treats the access modifier as a "normal" access rule. e.g. when there is no objectId/classId in the target function call, or when you want access rules to trigger uniformly regardless of the Object/Class involved.

**Access Rule Building**

* Access rules are built and deployed in separate MUD Systems.
* Access rule functions can ONLY be implemented as `view`/`pure` (`staticcall`) functions.
* Access rule functions must be public and accept the following parameters:
  * `uint256 entityId` - the `entityId` passed into the `access()` modifier from the target function call
  * `bytes memory targetCallData` - the `calldata` containing the parameter values of the target function call
* Access rule functions MUST NOT return any values and SHOULD revert when an access rule fails.

**Access Configuration/Enforcement**

* Target system (the System to enforce access upon) must be registered in the MUD World
* Access system (the System housing the access logic itself) must be registered in the MUD World
* Only Target System namespace owners can configure `configureAccess`/`setAccessEnforcement` for their Systems/functions
* Access configuration simply maps the target function to an access function that is to be called when access enforcement is enabled
* Access Enforcement toggles the access enforcement flag on/off to set the access rule in an active state (or not)

**Usage**

Adding the access modifier to a function:

{% code title="MySystem.sol" %}
```solidity
contract MySystem {
  function myFunction(uint256 objectId, uint256 param2) access(objectId) public {
    // do object access restricted stuff here
    MyObjectTable.setParam2(objectId, param2);
  }
}
```
{% endcode %}

A simple access rule implementation:

{% code title="MyAccessSystem.sol" %}
```solidity
contract MyAccessSystem {
    function myAccessFunction(uint256 objectId, bytes memory myFunctionParamData) public view {
        if (objectId != ERC721.owner(objectId)) {
            revert MyAccess_NotTokenOwner();
        }
        (,uint256 param2) = abi.decode(myFunctionParamData, (uint256, uint256));
        if (param2 != 2) {
            revert MyAccess_Param2RequirementNotMet();
        }
    }
}
```
{% endcode %}

Configuring Access Control:

{% code title="configureAccess call" %}
```solidity
// Configure access for a system function (configuring always re-sets the enforcement flag to false)
world.call( // caller must be the namespace-owner of MySystem's namespace
    accessConfigSystemId,
    abi.encodeCall(
        IAccessConfigSystem.configureAccess,
        (
            mySystemId,
            IMySystem.myFunction.selector,
            myAccessSystemId,
            IMyAccessSystem.myAccessFunction.selector
        )
    )
);
```
{% endcode %}

Managing Access Enforcement:

{% code title="setAccessEnforcement calls" %}
```solidity
// Enable access enforcement
world.call( // caller must be the namespace-owner of MySystem's namespace
    accessConfigSystemId,
    abi.encodeCall(
        IAccessConfigSystem.setAccessEnforcement,
        (mySystemId, IMySystem.myFunction.selector, true)
    )
);

// Disable access enforcement
world.call( // caller must be the namespace-owner of MySystem's namespace
    accessConfigSystemId,
    abi.encodeCall(
        IAccessConfigSystem.setAccessEnforcement,
        (mySystemId, IMySystem.myFunction.selector, false)
    )
);
```
{% endcode %}

Further descriptions such as Class/Object Tagging, System scoping, and the standalone RoleManagementSystem, can be found here: https://github.com/projectawakening/world-chain-contracts/tree/develop/mud-contracts/smart-object-framework-v2
