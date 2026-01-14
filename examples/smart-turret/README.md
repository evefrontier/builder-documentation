# Smart Turret

## Introduction

This guide will walk you through the process of building contracts for the [**Smart Turret**](/broken/pages/126b9890a0178019a4d01ff74a079abd036439e7) example, deploying them into an existing world running in Docker, and testing their functionality by executing scripts.

## Example Functionality

This example alters the Smart Turret to have two specific behaviors:

{% stepper %}
{% step %}
### Does not shoot at a specified corporation

The turret will not shoot at anyone in the specified corporation.
{% endstep %}

{% step %}
### Prioritizes ships with the lowest percentage of health

The turret prioritizes shooting ships that have the lowest percentage of health. This strategy allows ships to be destroyed faster. A byproduct is that groups of Smart Turrets will share targets if in range when several are used with this example.
{% endstep %}
{% endstepper %}

{% hint style="info" %}
🔧 Technical Note: The game processes targets in reverse array order from calling the inProximity function. While the weight value is used for sorting, it's not currently used in-game targeting logic.
{% endhint %}

## Pre-Requisites

Make sure you have set up your tools with [this guide](/broken/pages/913027044a4553eb71a12e64225ec7607419b983) and your world if you are testing locally with [this guide](/broken/pages/47a9cdeaeed0c84937ad7bfed5981b8ece2e6e51).
