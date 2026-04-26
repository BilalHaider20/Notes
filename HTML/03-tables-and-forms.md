# 03. Tables and Forms

## Tables
Tables are used to show **tabular data**.
Do not use tables for page layout.

### Basic table structure
```html
<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Ali</td>
      <td>25</td>
    </tr>
  </tbody>
</table>
```

## Table tags
- `table` = table container
- `thead` = header section
- `tbody` = body section
- `tr` = table row
- `th` = table header cell
- `td` = table data cell

## Forms
Forms collect user data.
This is a very common interview topic.

### Basic form example
```html
<form action="/submit" method="post">
  <label for="name">Name</label>
  <input type="text" id="name" name="name" />

  <button type="submit">Submit</button>
</form>
```

## Important form tags
- `form`
- `label`
- `input`
- `textarea`
- `select`
- `option`
- `button`

## Common input types
- `text`
- `email`
- `password`
- `number`
- `date`
- `checkbox`
- `radio`
- `file`
- `submit`

## `id` vs `name`
### `id`
Used to uniquely identify an element on the page.
Also connects `label` with `input`.

### `name`
Used when form data is submitted.
The backend usually reads values by `name`.

## Why `label` is important
```html
<label for="email">Email</label>
<input type="email" id="email" name="email" />
```
It improves usability and accessibility.
Clicking the label can focus the input.

## Placeholder vs value
### `placeholder`
Shows hint text inside the field.

### `value`
Sets the actual current value of the input.

## Validation attributes
- `required`
- `min`
- `max`
- `maxlength`
- `pattern`

Example:
```html
<input type="email" required />
```

## Radio vs checkbox
### Radio
Used when only one option can be selected.

### Checkbox
Used when multiple options can be selected.

## GET vs POST
### GET
- sends data in URL
- used for fetching/searching
- less secure for sensitive data

### POST
- sends data in request body
- used for submitting data
- better for sensitive or large data

## `button` vs `input type="submit"`
Both can submit forms, but `button` is more flexible because it can contain text or HTML.

## Easy interview questions
### Why should tables not be used for layout?
Because tables are meant for tabular data, not page structure.

### Why is `label` important in forms?
It improves accessibility and user experience.

### What is the difference between GET and POST?
GET sends data in URL, POST sends data in request body.

### What is the difference between `id` and `name` in forms?
`id` identifies the element in the page, `name` is used when submitting form data.

## Quick revision
- Use tables only for data
- Forms are very important for interviews
- Connect every label properly
- Understand GET, POST, validation, and input types
