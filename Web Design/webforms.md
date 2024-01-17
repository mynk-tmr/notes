## Markup

`form method='url' get='post'` 
divide with `section`
use `fieldset` to group related fields & `legend` for headings (a11y)
`aria-label` on label e.g. * (`required`)
Use <output> to display selected value in `range`


```html
<textarea rows='8' cols='10' resize='none' wrap='hard'>
  Placeholder <!-- if hard, send text with wrappings -->
</textarea>

<!-- radio & checkbox ; send nothing if none checked -->
<input type='radio | checkbox' name='common_id' checked>

<!-- drop-down -->
<select multiple> 
  <optgroup label='subheading'>
    <option selected value='1'>Banana</option> <!-- value=text by default --> 

<!-- track bars ; they are inline elements -->
<progress value="32" max="100">32%</progress> <!-- fallback child -->
<meter min max low optimum high > <!-- divide range & color codes it -->

<!-- range -->
<input type='range' min max step>


<!-- single fields -->
<input type='url| text | password | email | hidden'> 
<input type='image' src='pp.jpg' alt='click'> // sends coords in img (name.x=123&name.y=456)
<input type='file' accept='audio/*, .json' capture='user'> //if 'environment' uses plugged in
<input type='search'> //saves value for site

<!-- supported attrs -->
<input multiple spellcheck='true' inputmode='tel' maxlength='30' minlength pattern >


<!-- numeric input types -->
<input type='number' min max step='2 | any'> <!-- any enable float -->
<!-- others -->
`tel`
`date` `time` : (no timezone)
`datetime-local` : timezone
`month` : month + year
`week` : week + year
`color` : color-picker

```
**Other attributes**

`autocomplete` `autofocus`
`disabled` : disable the control (NOT validated or submitted)
`readonly`: validated and submitted
`form='form_id'` : associate with a form
`formaction` : URL for submission on submit and image buttons


## Styling

```css 

input[type="range"] {
  appearance: slider-vertical;
}

input[type='text'] {
  caret-color: red;
}

meter::-webkit-meter-suboptimum-value {
  background-color: red;
}

meter::-webkit-meter-optimum-value {
  background-color: #0f0;
}

```

Psuedo-selectors 

```css
:enabled :disabled :read-only :read-write :placeholder-shown 
:auto-fill :required :optional :blank

:valid :invalid :indeterminate :user-invalid /* ignore browser invalid */

:in-range :out-of-range :checked :default

input::placeholder
input[type='file']::-webkit-file-upload-button

```

Submit buttons attributes 
- formaction: Override the action attribute of the form.
- formenctype: Override the enctype attribute of the form.
- formmethod: Override the method attribute of the form.
- formnovalidate: Override the novalidate attribute of the form.
- formtarget: Override the target attribute of the form.



## Custom form controls




## Good UI / UX

- Aim for inline validations and show errors near field
- Avoid tooltips 
- Don't disable the submit button
- Show positive feedback too (green checks)
- Add Iconography to feedbacks
- Don't use Modals
- Show allowed values pattern right away

Common Regex : https://www.html5pattern.com/Names

Regex tutorial : https://www.sitepoint.com/demystifying-regex-with-practical-examples/