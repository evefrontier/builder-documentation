# How to Contribute
{% hint style="info" %}
This site is synced to GitHub. You can propose changes from any page. Use **Edit on GitHub** to open a pull request.
{% endhint %}

### What you can contribute

* Fix typos and broken links.
* Clarify steps that are confusing or missing.
* Add examples, screenshots, or diagrams.
* Update docs when behavior changes.
* Add troubleshooting notes and known pitfalls.

{% hint style="info" %}
If you are unsure, submit a small PR first. We can iterate from there.
{% endhint %}

### Quick workflow (recommended)

{% stepper %}
{% step %}
#### Open the page you want to change

Navigate to the exact page in the docs. Scroll to the section you want to improve.
{% endstep %}

{% step %}
#### Click **Edit on GitHub**

Open the page menu (top-right dropdown). Select **Edit on GitHub**. Sign in to GitHub if prompted. GitHub will open the source file for that page.
{% endstep %}

{% step %}
#### Edit in GitHub

Click the pencil icon to edit the file. If you do not have access, GitHub will fork first. Make your changes in the editor. Use the preview tab to sanity check formatting.
{% endstep %}

{% step %}
#### Propose the change

Scroll to the bottom of the editor. Write a clear commit message. Keep it short and specific. Select the option to create a new branch. Use a descriptive branch name. Click **Propose changes**.
{% endstep %}

{% step %}
#### Open a pull request

GitHub will suggest creating a PR. Fill in:

* **Title**: what changed.
* **Description**: why it changed.
* **Scope**: which pages or sections.
* **Verification**: how you checked accuracy.

Then click **Create pull request**.
{% endstep %}

{% step %}
#### Respond to review

You may get comments or requested edits. Update the same branch. GitHub will update the PR automatically.
{% endstep %}

{% step %}
#### Merge and publish

After approval, a maintainer will merge the PR. This site will sync and publish shortly after. If you do not see updates, wait a few minutes.
{% endstep %}
{% endstepper %}

### Best practices for strong PRs

#### Keep changes small

Prefer one topic per PR. Avoid drive-by refactors. Large rewrites are hard to review.

#### Write for the reader

State prerequisites early. Use concrete values and examples. Prefer copy-pasteable snippets. Call out footguns and failure modes.

#### Match existing style

Keep sentences short. Use headings to break up long pages. Use consistent terms across pages.

#### Be precise and verifiable

Do not guess. If something is unconfirmed, label it clearly. Add a link to a source when possible.

#### Don’t leak secrets

Never commit API keys, tokens, or internal URLs. Redact logs that include credentials.

### Courtesy guidelines

* Assume good intent.
* Be kind and direct in review comments.
* Ask before making broad structural changes.
* Don’t “win” arguments by rewriting someone’s work.
* Prefer suggestions over mandates.
* Be patient with response times.

{% hint style="warning" %}
If your change impacts behavior, call it out. Surprise changes cause real user pain.
{% endhint %}

### Common reasons PRs get delayed

* Mixed, unrelated edits in one PR.
* Vague commit message or PR description.
* Claims without a way to verify.
* Breaking links or inconsistent terminology.
* Big rewrites without prior discussion.

### Need help?

If GitHub is blocking your edit, copy the error message. Include it in your PR or message to the maintainers. If you are unsure where to edit, link the page.
