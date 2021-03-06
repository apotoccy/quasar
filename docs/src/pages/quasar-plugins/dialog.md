---
title: Dialog Plugin
---

Quasar Dialogs are a great way to offer the user the ability to choose a specific action or list of actions. They also can provide the user with important information, or require them to make a decision (or multiple decisions).

From a UI perspective, you can think of Dialogs as a type of floating modal, which covers only a portion of the screen. This means Dialogs should only be used for quick user actions only.

::: tip
Dialogs can also be used as a component in your Vue file templates (for complex use-cases, like specific form components with validation, selectable options, etc.). For this, go to [QDialog](/vue-components/dialog) page.
:::

The advantage of using Dialogs as Quasar Plugins as opposed to as Components is that the plugin can also be called from outside of Vue space and doesn't requires you to manage their templates. But as a result, their customization cannot be compared to their component counterpart.

## Installation
<doc-installation plugins="Dialog" />

## Usage
::: tip
For all the examples below, also see the browser console while you check them out.
:::

::: warning
This is not an exhaustive list of what you can do with Dialogs as Quasar Plugins. For further exploration check out the API section.
:::

<doc-example title="Basic" file="Dialog/Basic" />

<doc-example title="Radios, Checkboxes, Toggles" file="Dialog/Pickers" />

<doc-example title="Other options" file="Dialog/OtherOptions" />

## API
<doc-api file="Dialog" />
