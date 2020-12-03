### CKeditor
```html
<div id="editor"> 
  <p>Here goes the initial content of the editor.</p> 
</div>
```
```javascript
let editor;
tdcore.cke5.ClassicEditor
    .create( document.querySelector( '#editor' ) )
    .then( newEditor => {
        editor = newEditor;
    } )
    .catch( error => {
        console.error( error );
    } );
```
#### Get data
```javascript
editor.getData();
```
#### Set data
```javascript
editor.setData(data...);
```