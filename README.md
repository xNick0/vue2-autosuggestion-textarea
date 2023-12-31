# vue2-autosuggestion-textarea

## Usage

Use the command below to install the component:
```
npm install 0xNick/vue2-autosuggestion-textarea --save
```

Then import the js and css file:

```
import SuggestionArea from '@0xnick/vue2-autosuggestion-textarea';
import '@0xnick/vue2-autosuggestion-textarea/dist/v2-SuggestionArea.common;
import '@0xnick/vue2-autosuggestion-textarea/dist/v2-SuggestionArea.css';
```

I called it `SuggestionArea`, you can call it however you want.

## Props

| Name | Description |
| :---: | :---: | 
| options | Use this property as your list of options you want to be displayed |

---

## Classes CSS

| Class | Description |
| :---: | :---: | 
| .suggested-elements | This is the class assigned to the dropdown |
| .suggestion-area | This is the class asssigned to the textarea |

---

## Events

There are no events available.


## Features

> You can press Up or Down to iterate through the list
>
> You can press TAB to autocomplete with the selected item

![Live test](/media/live_test.gif)