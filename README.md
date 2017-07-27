# react-text-mask-hoc · [![npm](https://img.shields.io/npm/v/react-text-mask-hoc.svg)](https://npm.im/react-text-mask-hoc)

> A higher-order [text-mask](https://github.com/text-mask/text-mask)
component for [React](https://facebook.github.io/react/) and
[React Native](https://facebook.github.io/react-native/).

`text-mask` is great. It's a feature-rich solution for creating input
masks for various libraries and frameworks. However the React
implementation has some long-standing bugs and feature requests that
bury the potential of the library.

__Features:__

- Supports all features from `text-mask`, see its
[documentation](https://github.com/text-mask/text-mask/blob/master/componentDocumentation.md)
for more information.
- __Custom components:__ you can mask any components through a simple
[adapter](#adapters) interface!
- __Platform agnostic:__ works in all browsers, React Native and Node.js
(useful for server-side rendering)!

## Table of Contents

- [Install](#install)
- [Usage](#usage)
- [Examples](#examples)
- [API](#api)
  - [`createMaskedComponent`](#createmaskedcomponentwrappedcomponent)
  - [Adapters](#adapters)
    - for React: `InputAdapter` and `SpanAdapter`
    - for React Native: `TextInputAdapter` and `TextAdapter`
  - [`TextMaskElement`](#textmaskelement)

## Install

```bash
yarn add react-text-mask-hoc
  # or
npm install --save react-text-mask-hoc
```

## Usage

```jsx
import React, {PureComponent} from 'react';
import {createMaskedComponent, InputAdapter} from 'react-text-mask-hoc';

// You can provide your own adapter component or use one of included in the library.
const MaskedInput = createMaskedComponent(InputAdapter);

export default () =>
    <MaskedInput
        mask={['(', /[1-9]/, /\d/, /\d/, ')', ' ', /\d/, /\d/, /\d/, '-', /\d/, /\d/, /\d/, /\d/]}
        guide={false}
        value="5554953947"
    />;
```

To use in React Native import `react-text-mask-hoc/ReactNative` instead:

```jsx
import {createMaskedComponent, TextInputAdapter} from 'react-text-mask-hoc/ReactNative';

const MaskedInput = createMaskedComponent(TextInputAdapter);
```

## Examples

- [React](https://github.com/Vlad-Zhukov/react-text-mask-hoc/tree/master/examples/reactapp/)
- [React Native](https://github.com/Vlad-Zhukov/react-text-mask-hoc/tree/master/examples/reactnativeapp/)

## API

### `createMaskedComponent(AdaptedComponent)`

A [HOC](https://facebook.github.io/react/docs/higher-order-components.html)
that grants `text-mask` functionality to the wrapped component.

It's an uncontrolled component and it maintains its own state, however
it can also be controlled with props.

__Arguments__

1. `AdaptedComponent` _(React.Component)_: A React component that
follows the [adapter](#adapters) specification.

__Returns__

A React component that accepts the following props:

- all [`text-mask` settings](https://github.com/text-mask/text-mask/blob/master/componentDocumentation.md)
- `[value]` _(String|Number)_: A value that will be masked. Works as
the default value by default, and can be used to update the masked
value at any time. Defaults to `''`.
- `[onChange]` _(Function)_: A function that is called on input changes.
Takes 2 arguments: the native `event` (varies from a platform) and
the next state (has `value` and `caretPosition` properties).
- `[componentRef]` _(Function)_: A function that is called with a
reference to the `WrappedComponent`.

See [the source code](https://github.com/Vlad-Zhukov/react-text-mask-hoc/blob/master/src/createMaskedComponent.js)
for a list of props it takes.

---

### Adapters

Adapters are React components that implement a special interface for the
[`createMaskedComponent`](#createmaskedcomponentwrappedcomponent).

List of adapters included in this library:

- for React
  - `InputAdapter`
  - `SpanAdapter`
- for React Native
  - `TextInputAdapter`
  - `TextAdapter`

__Specification__

An adapter must be a React component that takes `value`, `caretPosition`
and `onChange` props, and exposes a `caretPosition` getter that always
returns a positive integer number.

See [ReactAdapters.js](https://github.com/Vlad-Zhukov/react-text-mask-hoc/blob/master/src/ReactAdapters.js)
and [ReactNativeAdapters.js](https://github.com/Vlad-Zhukov/react-text-mask-hoc/blob/master/src/ReactNativeAdapters.js)
for examples.

---

### `TextMaskElement`

A class that calculates a value and a caret position. Based on the
`createTextMaskInputElement()` from [`text-mask-core`](https://github.com/text-mask/text-mask/tree/master/core).

Exported for testing purposes only.

---
