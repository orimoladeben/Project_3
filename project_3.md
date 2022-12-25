`sudo apt update`

`sudo apt upgrade`

`curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash `

`sudo apt-get install -y nodejs`

`node -v `

`npm -v `

![node and npm successfully installed](./images/node%20and%20npm%20installed.PNG)

`mkdir Todo`

`ls -lih`
![Todo directory created](./images/Todo%20directory%20created.PNG)

`npm init`
![npm init guide](./images/npm%20init%20guide.PNG)

`ls`

![package json](./images/package%20json%20confirmation.PNG)

`npm install express`

`touch index.js`
![idex js created](./images/index%20js%20confirm.PNG)


`npm install dotenv`

`vim index.js`


`const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use((req, res, next) => {
res.send('Welcome to Express');
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});`


`node index.js`
![running on port 5000 confirm](./images/port%205000%20running%20confirmation.PNG)

`http://<PublicIP-or-PublicDNS>:5000
`
![welcome to express webpage](./images/welcome%20to%20express%20webpage.PNG)

`mkdir routes`

`cd routes`

`touch api.js`

`vim api.js`

`const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;`

![terminal step2 end](./images/terminal%20step2%20end.PNG)

`cd`

`cd Todo`

`npm install mongoose`
![mongoose install](./images/install%20mongoose.PNG)

`mkdir models`

`cd models`

`touch todo.js`

`vim todo.js`

`const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;`

![todo js terminal1](./images/Todo%20js%20step4%20terminal1.PNG)

`cd routes`

`vim api.js`

`:%d`

`const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});

router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})

module.exports = router;`

![route api js terminal2](./images/route%20api%20js%20step4%20terminal2.PNG)

`mongodb webpage`

![mongodb webpage](./images/mondb%20webpage1.PNG)

`mongodb connect link`

![mongodb connection link](./images/mondb%20connect%20webpage.PNG)

`touch .env`

`vi .env`

`DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'`

![env terminal pic](./images/env%20terminal.PNG)

`vim index.js`

`const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
console.log(err);
next();
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});`

`node index.js`
![database connected successfully](./images/database%20connected%20successfully.PNG)

`POST request on postman`
![POST request on Postman](./images/POST%20request%20on%20postman.PNG)

`GET request on postman`
![GET request on postman](./images/GET%20request%20postman.PNG)

` npx create-react-app client`
![create react client](./images/create%20react%20client.PNG)

`npm install concurrently --save-dev`
![concurrently installed](./images/concurrently%20installed.PNG)

`npm install nodemon --save-dev`
![nodemon install](./images/nodeman%20installed.PNG)

`vim package.json`

`{
  "name": "todo",
  "version": "1.0.0",
  "description": "A todo app",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "start-watch": "nodemon index.js",
    "dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
    },
  "keywords": [
    "todo",
    "application"
  ],
  "author": "Ben Orimolade",
  "license": "ISC",
  "dependencies": {
    "dotenv": "^16.0.3",
    "express": "^4.18.2",
    "mongoose": "^6.8.1"
  },
  "devDependencies": {
    "concurrently": "^7.6.0",
    "nodemon": "^2.0.20"
  }
}`

![edit package json](./images/edit%20package%20json.PNG)

`cd client`

`vi package.json`

![local proxy added](./images/local%20proxy%20added.PNG)

`npm run dev`
![react started 3000](./images/react%20started%203000.PNG)

`cd client`

`cd src`

`mkdir components`

`touch Input.js ListTodo.js Todo.js`

`vi Input.js`

`import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {

state = {
action: ""
}

addTodo = () => {
const task = {action: this.state.action}

    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }

}

handleChange = (e) => {
this.setState({
action: e.target.value
})
}

render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>
)
}
}

export default Input`

![terminal pic Input js](./images/terminal%20pic%20Input%20js%20edit.PNG)

`npm install axios`
![axios install in client folder](./images/Axios%20installed.PNG)

`cd src/components`

`vi ListTodo.js`

`import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {

return (
<ul>
{
todos &&
todos.length > 0 ?
(
todos.map(todo => {
return (
<li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
)
})
)
:
(
<li>No todo(s) left</li>
)
}
</ul>
)
}

export default ListTodo`

`vi Todo.js`

`import React, {Component} from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

state = {
todos: []
}

componentDidMount(){
this.getTodos();
}

getTodos = () => {
axios.get('/api/todos')
.then(res => {
if(res.data){
this.setState({
todos: res.data
})
}
})
.catch(err => console.log(err))
}

deleteTodo = (id) => {

    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))

}

render() {
let { todos } = this.state;

    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )

}
}

export default Todo;`

`cd ..`

`vi App.js`

`import React from 'react';

import Todo from './components/Todo';
import './App.css';

const App = () => {
return (
<div className="App">
<Todo />
</div>
);
}

export default App;`


`vi App.css`

`.App {
text-align: center;
font-size: calc(10px + 2vmin);
width: 60%;
margin-left: auto;
margin-right: auto;
}

input {
height: 40px;
width: 50%;
border: none;
border-bottom: 2px #101113 solid;
background: none;
font-size: 1.5rem;
color: #787a80;
}

input:focus {
outline: none;
}

button {
width: 25%;
height: 45px;
border: none;
margin-left: 10px;
font-size: 25px;
background: #101113;
border-radius: 5px;
color: #787a80;
cursor: pointer;
}

button:focus {
outline: none;
}

ul {
list-style: none;
text-align: left;
padding: 15px;
background: #171a1f;
border-radius: 5px;
}

li {
padding: 15px;
font-size: 1.5rem;
margin-bottom: 15px;
background: #282c34;
border-radius: 5px;
overflow-wrap: break-word;
cursor: pointer;
}

@media only screen and (min-width: 300px) {
.App {
width: 80%;
}

input {
width: 100%
}

button {
width: 100%;
margin-top: 15px;
margin-left: 0;
}
}

@media only screen and (min-width: 640px) {
.App {
width: 60%;
}

input {
width: 50%;
}

button {
width: 30%;
margin-left: 10px;
margin-top: 0;
}
}`


`cd ../..`

`npm run dev`

![final terminal pic](./images/final%20terminal%20pic.PNG)

![final Todo successfully created](./images/Final%20Todo%20app%20successful.PNG)
