# VueTypeahead

See a live demo [here](http://pespantelis.github.io/vue-typeahead/).

## Install

#### NPM
Available through npm as `vue-typeahead`.
```
npm install --save vue-typeahead
```
> Also, you need to install a HTTP client like [`axios`](https://github.com/mzabriskie/axios).

## Usage

```html
<template>
    <div>
        <!-- optional indicators -->
        <i class="fa fa-spinner fa-spin" v-if="loading"></i>
        <template v-else>
            <i class="fa fa-search" v-show="isEmpty"></i>
            <i class="fa fa-times" v-show="isDirty" @click="reset"></i>
        </template>

        <!-- the input field -->
        <input type="text"
               placeholder="..."
               autocomplete="off"
               v-model="query"
               @keydown.down="down"
               @keydown.up="up"
               @keydown.enter="hit"
               @keydown.esc="reset"
               @blur="reset"
               @input="update"/>

        <!-- the list -->
        <ul class="type-ahead-menu" v-show="hasItems">
            <!-- for vue@1.0 use: ($item, item) -->
            <li v-for="(item, $item) in items" :class="activeClass($item)" @mousedown="hit"
                @mousemove="setActive($item)">
                <span v-text="item.name"></span>
            </li>
        </ul>

    </div>
</template>

<script>
    import VueTypeahead from 'vue-typeahead'

    export default {
        extends: VueTypeahead,

        props: {
            // The source url
            src: {
                required: true,
                type: String
            },

            // Limit the number of items which is shown at the list
            limit: {
                type: Number,
                default: 5
            },

            // The minimum character length needed before triggering
            minChars: {
                type: Number,
                default: 3
            },

            // Highlight the first item in the list
            selectFirst: {
                type: Boolean,
                default: false
            },

            // Override the package value ("q") of query parameter name
            // Use a falsy value for RESTful query
            queryParamName: {
                type: String,
                default: 'search_string'
            }
        },

        data: function() {
            return {
                // The data that would be sent by request
                data: {},
            }
        },

        methods: {
            // The callback function which is triggered when the user hits on an item
            // (required)
            onHit(item) {
                // alert(item)
                console.log(item)
            },

            // The callback function which is triggered when the response data are received
            // (optional)
            prepareResponseData(data) {
                // data = ...
                return data
            }
        }
    }
</script>

<style scoped>
    ul.type-ahead-menu {
        position: absolute;
        list-style-type: none;
        margin: 0;
        margin-block-start: 0;
        margin-block-end: 0;
        margin-inline-start: 0;
        margin-inline-end: 0;
        padding-inline-start: 0;

        border: 1px solid #cccccc;
        background-color: white;
    }

    ul.type-ahead-menu li {
        border-bottom: 1px solid #cccccc;
    }

    ul.type-ahead-menu li > * {
        display: block;
    }

    ul.type-ahead-menu li.active {
        background-color: #dedede;
    }
</style>
```

## Key Actions
**Down Arrow:** Highlight the previous item.

**Up Arrow:** Highlight the next item.

**Enter:** Hit on highlighted item.

**Escape:** Hide the list.

## States
**loading:** Indicates that awaits the data.

**isEmpty:** Indicates that the input is empty.

**isDirty:** Indicates that the input is not empty.
> Useful if you want to add icon indicators (see the demo)

## License
VueTypeahead is released under the MIT License. See the bundled LICENSE file for details.
