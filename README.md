// In your build.gradle: implementation("io.coil-kt:coil-compose:2.5.0")

@Composable
fun FastImageItem(imageUrl: String) {
    AsyncImage(
        model = ImageRequest.Builder(LocalContext.current)
            .data(imageUrl)
            .crossfade(true) // Smooth transition looks "faster" to the user
            .diskCachePolicy(CachePolicy.ENABLED) // Saves to phone storage
            .memoryCachePolicy(CachePolicy.ENABLED) // Saves to RAM for instant scrolling
            .build(),
        contentDescription = "Optimized Image",
        modifier = Modifier
            .fillMaxWidth()
            .height(200.dp),
        contentScale = ContentScale.Crop,
        // Shows a tiny "blur" or color while the high-res image loads
        placeholder = painterResource(R.drawable.placeholder_blur) 
    )
}
document.addEventListener('DOMContentLoaded', function() {
  playground('code'); // attach to all <code> elements
});


// ES6
import playground from 'kotlin-playground';

document.addEventListener('DOMContentLoaded', () => {
  playground('code'); // attach to all <code> elements
});
```

### Use from plugins

1) [Kotlin Playground WordPress plugin](https://github.com/Kotlin/kotlin-playground-wp-plugin) — [WordPress](https://wordpress.com/) plugin which allows to embed interactive Kotlin playground to any post.
2) [Kotlin Playground Coursera plugin](https://github.com/AlexanderPrendota/kotlin-playground-coursera-plugin) — Allows embedding interactive Kotlin playground for [coursera](https://www.coursera.org/) lessons.
3) [Kotlin Playground Orchid plugin](https://orchid.netlify.com/plugins/OrchidSyntaxHighlighter#kotlin-playground) — Allows embedding interactive Kotlin playground in [Orchid](https://orchid.netlify.com/) documentation sites.

### Options

Kotlin Playground supports several events, and also Kotlin version or server URL overwriting passing an additional `options` parameter on initialisation.

For example:
```js
function onChange(code) {
  console.log("Editor code was changed:\n" + code);
}

function onTestPassed() {
   console.log("Tests passed!");
}

const options = {
  server: 'https://my-kotlin-playground-server',
  version: '1.3.50',
  onChange: onChange,
  onTestPassed: onTestPassed,
  callback: callback(targetNode, mountNode)
};

playground('.selector', options)

```

**Events description:**

- `onChange(code)` — Fires every time the content of the editor is changed. Debounce time: 0.5s.
 _code_ — current playground code.

- `onTestPassed` — Is called after all tests passed. Use for target platform `junit`.

- `onTestFailed` — Is called after all tests failed. Use for target platform `junit`.

- `onCloseConsole` — Is called after the console's closed.

- `onOpenConsole` — Is called after the console's opened.

- `getJsCode(code)` — Is called after compilation Kotlin to JS. Use for target platform `js`.
   _code_ — converted JS code from Kotlin.

- `callback(targetNode, mountNode)` — Is called after playground's united.
 _targetNode_ — node with plain text before component initialization.
 _mountNode_  — new node with runnable editor.

- `getInstance(instance)` - Getting playground state API.

  ```js
  instance.state      // playground attributes, dependencies and etc.
  instance.nodes      // playground NodeElement.
  instance.codemirror // editor specification.
  instance.execute()  // function for executing code snippet.
  instance.getCode()  // function for getting code from snippet.
  ```

## Customizing editors


Use the following attributes on elements that are converted to editors to adjust their behavior.

- `data-version`: Target Kotlin [compiler version](https://api.kotlinlang.org/versions):

   ```html
    <code data-version="1.0.7">
    /*
    Your code here
    */
    </code>
    ```
- `args`: Command line arguments.

  ```html
  <code args="1 2 3">
  /*
  Your code here
  */
  </code>
  ```

- `data-target-platform`: target platform: `junit`, `canvas`, `js` or `java` (default).

  ```html
   <code data-target-platform="js">
    /*
    Your code here
    */
   </code>
   ```
- `data-highlight-only`: Read-only mode, with only highlighting. `data-highlight-only="nocursor"` - no focus on editor.

  ```html
  <code data-highlight-only>
    /*
    Your code here
    */
  </code>
  ```

  Or, you can make only a part of code read-only by placing it between `//sampleStart` and `//sampleEnd` markers.
  If you don't need this just use attribute `none-markers`.
  For adding hidden files: put files between `<textarea>` tag with class `hidden-dependency`.

  ```html
  <code>
  import cat.Cat

  fun main(args: Array<String>) {
  //sampleStart
      val cat = Cat("Kitty")
      println(cat.name)  
  //sampleEnd                 
  }
    <textarea class="hidden-dependency">
      package cat
      class Cat(val name: String)
    </textarea>
  </code>
  ```
  Also if you want to hide code snippet just set the attribute `folded-button` to `false` value.

- `data-js-libs`: By default component loads jQuery and makes it available to the code running in the editor. If you need any additional JS libraries, specify them as comma-separated list in this attribute.

  ```html
  <code data-js-libs="https://my-awesome-js-lib/lib.min.js">
    /*
    Your code here
    */
   </code>
  ```

- `auto-indent="true|false"`: Whether to use the context-sensitive indentation. Defaults to `false`.

- `theme="idea|darcula|default"`: Editor IntelliJ IDEA themes.

- `mode="kotlin|js|java|groovy|xml|c|shell|swift|obj-c"`: Different languages styles. Runnable snippets only with `kotlin`. Default to `kotlin`.

- `data-min-compiler-version="1.0.7"`: Minimum target Kotlin [compiler version](https://api.kotlinlang.org/versions)

- `data-autocomplete="true|false"`: Get completion on every key press. If `false` => Press ctrl-space to activate autocompletion. Defaults to `false`.

- `highlight-on-fly="true|false"`: Errors and warnings check for each change in the editor. Defaults to `false`.

- `indent="4"`: How many spaces a block should be indented. Defaults to `4`.

- `lines="true|false"`: Whether to show line numbers to the left of the editor. Defaults to `false`.

- `from="5" to="10"`: Create a part of code. Example `from` line 5 `to` line 10.

- `data-output-height="200"`: Set the iframe height in `px` in output. Use for target platform `canvas`.

- `match-brackets="true|false"`: Determines whether brackets are matched whenever the cursor is moved next to a bracket. Defaults to `false`

- `data-crosslink="enabled|disabled"`: Show link for open in playground. Defaults to `undefined` – only supported in playground.

- `data-shorter-height="100"`: show expander if height more than value of attribute

- `data-scrollbar-style`: Chooses a [scrollbar implementation](https://codemirror.net/doc/manual.html#config). Defaults to `overlay`.

## Supported keyboard shortcuts

  - Ctrl+Space		   — code completion
  - Ctrl+F9/Cmd+R	       — execute snippet
  - Ctrl+/		       — comment code
  - Ctrl+Alt+L/Cmd+Alt+L   — format code
  - Shift+Tab		   — decrease indent
  - Ctrl+Alt+H/Cmd+Alt+H             — highlight code
  - Ctrl+Alt+Enter/Cmd+Alt+Enter    — show import suggestions


## Develop and contribute

1. Fork & clone [our repository](https://github.com/JetBrains/kotlin-playground).
2. Install required dependencies `yarn install`.
3. `yarn start` to start local development server at http://localhost:9000.
4. `yarn test` to run tests.
   `TEST_HEADLESS_MODE=true` to run tests in headless mode.
5. `yarn run build` to create production bundles.
