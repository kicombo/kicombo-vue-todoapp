# kicombo-vue-1
* A simple todoapp made with vue.
* Use Local Storage in browsers.
* Darkmode theme!

## == How to ==
use ```git clone``` to clone this repository to your computer. Then use ```npm install``` to download all dependencies. After that:
* Use ```npm run dev``` to run this in development mode
* Use ```npm run build``` to build a production mode directory called 'dist'

## == Notes == 

### === Webpack ===
Webpack is a beautiful tool that bundles your source files into one js file. Use it by downloading ```webpack``` & ```webpack-cli``` using ```npm i -D```

### === Vue Reactivity on referenced objects (especially arrays) ===

If a vue data references to an object, and that referenced object changes, vue can still detect that change. This is probably because the vue data does not only have the reference code as its value. And thus even though the reference code didn't change, vue can track the change in the referenced object. However, in the case of arrays(which is an object in Javascript), Vue only reacts to the change made by selected methods.

Working on this project, I was unaware of this fact. And this made me confused because of the line ```this.todos = this.db.getState().todos```, which returns an array. The array is saved as a reference by ```this.todo```. When I use ```this.todos.push(...)```, after using push() in this.db and this.todos, the array had the items pushed into it twice.

At first I thought that ```this.db.get('todos').push(newTodo).write()``` pushed the object not only to the local storage, but also to the array. And thus, the item was added twice to the array due to two push() being called. This was the right answer. However since I was unaware of the fact that Vue only reacts to selected methods, I thought this was not possible, because if the array was changed from the first line of code, Vue would have tracked the change and changed the list by its reactivity.

Next, I thought that the answer to the problem lied within ```this.db.getState().todos```. I thought this line of code returned a _strange_ array, an array that updates on the local storage's changes only when it's methods, like shift() or push(), is used. However this was also wrong since I found out, after some experiment, that the so-called _strange_ array updated to the local storage's change right away.

After this, I did further research and I found out that my first speculation was right. The answer to this problem is within how DOM update is triggered with arrays. Vue triggers DOM updates only for selected methods. However the method that lodash uses to update the array returned by ```this.db.getState().todos``` is not one of those selected methods and thus Vue does not react (trigger DOM update) to that.

Using the ```cloneDeep()``` method from lodash stops this problem from happening as it returns an array that is not effected by lodash's methods for changing local storage.

### === Lowdb / Lodash ===
Use Lowdb(which in turn uses Lodash) to connect to the Local Storage of the client(browser).
