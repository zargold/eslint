# disallow `&nbsp;` which is considered bad practice in HTML and can lead to rendering issues with React (no-nbsp)

Work around ever having to use `&nbsp;` the HTML hack for adding a space.

```js
// disallowed
document.body.innerHTML = `<div>blah:&nbsp;windows</div>`
```

should be rewritten as

```js
// preferred
document.body.innerHTML = `<div>blah: windows</div>`
// or
document.body.innerHTML = `<div>blah:
    <span style='padding: 1em'> windows</span>
</div>`
```

## Rule Details

This rule disallows `&nbsp;` to appear in JavaScript.

Examples of **incorrect** code for this rule:

```js
/*eslint no-nbsp: "error"*/

document.body.innerHTML = `<div>blah:&nbsp;windows</div>`

const myList = ['apple', 'blackberry', 'android', 'windows']
document.body.innerHTML = myList.join('&nbsp;')

// or react specific:
const MyDumbComponent = ({stuffs}) => (
    <div>
        {stuffs.map((stuff, i) =>
            <span key={stuff.id}>
                <span>{stuff.content}</span>&nbsp;
            </span>
        )}
    </div>
)
```

Examples of **correct** code for this rule:

```js
/* eslint no-nbsp: "error"*/


document.body.innerHTML = `<div>blah: windows</div>`

const myList = ['apple', 'blackberry', 'android', 'windows']
document.body.innerHTML = myList.join(' ')

// or react specific:
const MyDumbComponent = ({stuffs}) => (
    <div>
        {stuffs.map((stuff, i) =>
            <span key={stuff.id}>{`${stuff.content}${i < stuffs.length
                ? ' '
                : ''
            }`}</span>
        )}
    </div>
)

// or if all else fails:
const styling = document.createElement('style')
styling.appendChild(document.createTextNode('.padding-right { padding-right: 1em; }'))
document.head.appendChild(styling);
document.body.innerHTML = `<div><span class='padding-right'>blah:<span> windows</div>`
// or
// create a class in css that will add content: " " to &:after of something
<style>
.space-right::after {
    content: " "
}
</style>
document.body.innerHTML = `<div><span class='space-right'>blah:<span> windows</div>`
// for react specifically you can also use:
// <span>{' '}</span>
```

## When Not To Use It

If you really can't avoid it but there's basically always a way around using this HTML hack. But keep in mind it will cause rendering issues in React (at least pre-version 16 -- since I haven't updated yet) and will sometimes render Ä instead of a space or in addition to a space.
