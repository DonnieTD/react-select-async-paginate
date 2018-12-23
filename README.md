[![NPM](https://img.shields.io/npm/v/react-select-async-paginate.svg)](https://www.npmjs.com/package/react-select-async-paginate)

# react-select-async-paginate

Wrapper above `react-select` that supports pagination on menu scroll.

## Sandbox examples

- [Simple](https://codesandbox.io/s/o75rno2w65)
- [Multi](https://codesandbox.io/s/2323yrlo9r)
- [Initial options](https://codesandbox.io/s/q111nqw9j)

## Versions

| react-select | react-select-async-paginate |
|--------------|-----------------------------|
| 2.x | 0.2.x |
| 1.x | 0.1.x |

## Installation

```
npm install react-select react-select-async-paginate
```

or

```
yarn add react-select react-select-async-paginate
```

## Usage

`AsyncPaginate` is an alternative of `Select.Async` but supports loading page by page. It is wrapper above default `react-select` thus it accepts all props of default `Select` except `isLoading`. And there are some new props:

### loadOptions

Required. Async function that take two arguments:

1. Current value of search input.
2. Loaded options for current search.

It should return next object:

```
{
  options: [{ label: 'label', value: 'value' }, ...],
  hasMore: true/false,
}
```

It similar to `loadOptions` from `Select.Async` but there is some differences:

1. Loaded options as 2nd argument.
2. Not supports callback.
3. Should return `hasMore` for detect end of options list for current search.

### cacheUniq

Not required. Can take any value. When this prop changed, `AsyncPaginate` cleans all cached options.

### selectRef

Ref for take `react-select` instance.

## Example

```
import AsyncPaginate from 'react-select-async-paginate';

...

/*
 * assuming the API returns something like this:
 *   const json = {
 *     results: [
 *       {
 *         value: 1,
 *         label: 'Audi',
 *       },
 *       {
 *         value: 2,
 *         label: 'Mercedes',
 *       },
 *       {
 *         value: 3,
 *         label: 'BMW',
 *       },
 *     ],
 *     has_more: true,
 *   };
 */

async function loadOptions(search, loadedOptions) {
  const response = await fetch(`/awesome-api-url/?search=${search}&offset=${loadedOptions.length}`);
  const responseJSON = await response.json();

  return {
    options: responseJSON.results,
    hasMore: responseJSON.has_more,
  };
}

<AsyncPaginate
  value={value}
  loadOptions={loadOptions}
  onChange={setValue}
/>
```

## Replacing Components

Usage of replacing components is similar with `react-select`, but there is one difference. If you redefine `MenuList` you should wrap it with `wrapMenuList` for workaround of some internal bugs of `react-select`.

```
import AsyncPaginate, { wrapMenuList } from 'react-select-async-paginate';

...

const MenuList = wrapMenuList(CustomMenuList);

<AsyncPaginate
  {...otherProps}
  components={{
    ...otherComponents,
    MenuList,
  }}
/>
```
