# vue-bulma-draggable-input

## Project setup

### get project
```
yarn add https://github.com/looxis/vue-bulma-draggable-input

or

npm install --save https://github.com/looxis/vue-bulma-draggable-input
```
### import component
```
import DraggableInput from "vue-bulma-draggable-input";
```
### connect component and pass data to it 
#### Props
##### data
Type: `String`<br>
Required: `true`<br>
```
<draggable-input v-model="data"></draggable-input>
```
### connect bulma styles to your component
```
import '../node_modules/bulma/css/bulma.css';
```
