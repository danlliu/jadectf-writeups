# jsduck
## Rev (489 pts)

Writeup by danlliu (WolvSec)

## Challenge

We are given a web server, as well as the source code.

## Solution

First, we can look at `app.js`:

```js
// require('dotenv').config();
const express = require('express');
const app = express();
const bodyParser = require('body-parser');
const fs = require('fs');

app.set('etag', false);
app.use(bodyParser.urlencoded());
app.set('view engine', 'ejs');
app.use(express.static('public'));

let memory = [0];
const memorySize = 100;
const errorDecoded = 'Idiot!';
let pointer = 1;

let program = [62, 91, 43, 43, 46, 62, 93, 91, 60, 93, 62, 91, 43, 45, 43, 46, 62, 93];
let programPtr = 0;
let braces = [];

function resetMemory() {
    memory = [0];
    pointer = 1;

    program = [62, 91, 43, 43, 46, 62, 93, 91, 60, 93, 62, 91, 43, 45, 43, 46, 62, 93];
    programPtr = 0;
    braces = [];
}

function populateMemory(iv, l) {
    for ( let i = l-1; i >= 0; i-- ) {
        memory.push(iv[i].charCodeAt(0));
    }
    
    pointer = l+1;
    for ( let i = pointer; i < memorySize; i++ ) {
        memory.push(0);
    }

    pointer = 0;
}

let gg = {
    fun_0x60: function () {
        pointer--;
    },

    fun_0x62: function () {
        pointer++;
    },

    fun_0x43: function () {
        memory[pointer]++;
    },

    fun_0x45: function () {
        memory[pointer]--;
    },

    fun_0x91: function () {
        if( memory[pointer] == 0 ){
            while( program[programPtr] != "]" && programPtr <= program.length){
                programPtr += 1;
            }
        } else {
            braces.push(programPtr);
        }
    },

    fun_0x93: function () {
        if (braces.length > 0){
            programPtr = braces.pop();
            programPtr -= 1;
        }
    },

    fun_0xgg: function (s) {
        try {
            console.log(s);
            return JSON.parse(s);
        } catch ( e ) {
            return false;
        }
    }
}; 

function decode(encoded) {
    let l = encoded.length;
    if ( !l || l <= 0 || l >= memorySize ) {
        return errorDecoded;
    }

    populateMemory(encoded, l);
    let decoded = '';
    while ( programPtr < program.length ) {
        i = program[programPtr];
        if ( i == 46 ) {
            decoded += String.fromCharCode(memory[pointer]);
            programPtr++;
            continue;
        }
        
        (gg[`fun_0x${i}`])();
        if ( pointer >= memorySize ) {
            return errorDecoded;
        }
        programPtr++;
    }
    resetMemory();
    return gg['fun_0xgg'](decoded);
}

function getFlag() {
    return new Promise((resolve, reject) => {
        fs.readFile('/usr/app/flag.txt', (err, data) => {
            if ( err ) {
                reject(err);
            } else {
                resolve(data);
            }
        });
    });
}


app.get('/', (req, res) => {
    res.render('index', {
        message: ''
    });
});

app.post('/', async (req, res) => {
    try {
        console.log(req.body.code);
        let decoded = decode(req.body.code);
        console.log(decoded);
        if ( 
            decoded &&
            decoded.admin === 'true' &&
            decoded.guest === 'false' &&
            decoded.organization === 'cyberlabs'
        ) {
            res.render('index', {
                message: await getFlag()
            });
            return;
        }
        res.redirect('/');
    } catch ( error ) {
        console.log(error);
        res.redirect('/');
    }
});

app.listen(4000, () => console.log('Listening on', 4000));
```

Here, we see a dictionary called `gg`, which contains various functions, listed below:

```javascript
    fun_0x60: function () {
        pointer--;
    },

    fun_0x62: function () {
        pointer++;
    },

    fun_0x43: function () {
        memory[pointer]++;
    },

    fun_0x45: function () {
        memory[pointer]--;
    },

    fun_0x91: function () {
        if( memory[pointer] == 0 ){
            while( program[programPtr] != "]" && programPtr <= program.length){
                programPtr += 1;
            }
        } else {
            braces.push(programPtr);
        }
    },

    fun_0x93: function () {
        if (braces.length > 0){
            programPtr = braces.pop();
            programPtr -= 1;
        }
    },

    fun_0xgg: function (s) {
        try {
            console.log(s);
            return JSON.parse(s);
        } catch ( e ) {
            return false;
        }
    }
```

If we think of `pointer` as a pointer into memory, we see what these functions are doing:

- `fun_0x60` moves the pointer to the left
- `fun_0x62` moves the pointer to the right
- `fun_0x43` increments the data underneath the pointer
- `fun_0x45` decrements the data underneath the pointer
- `fun_0x91` begins a loop that runs as long as the data underneath the pointer is not 0
- `fun_0x93` is the end of the loop
- `fun_0xgg` parses a string as JSON, and is unrelated to the rest

This is an implementation of the language P''! (or, brain****, but I'm not going to try to cross compile to that). Let's cross compile the program to `P''`. Note that `46` servers to output to a string, which is later parsed as JSON.

```c
62, 91, 43, 43, 46, 62, 93, 91, 60, 93, 62, 91, 43, 45, 43, 46, 62, 93

--ptr;  // 62
while (*ptr) {  // 91
    ++*ptr;  // 43
    ++*ptr;  // 43
    output *ptr;  // 46
    ++ptr; // 62
}  // 93
while (*ptr) {  // 91
    --ptr;  // 60
}  // 93
// 62
while (*ptr) {  // 91
    ++*ptr;  // 43
    --*ptr;  // 45
    ++*ptr;  // 43
    output *ptr;  // 46
    ++ptr; // 62
}  // 93
```

By running `app.js` locally, with some outputs, we can trace out the program flow. But wait... why does the output in the third `while` loop never run?

Let's take a look at the implementation of `[`:

```javascript
fun_0x91: function () {
    if( memory[pointer] == 0 ){
        while( program[programPtr] != "]" && programPtr <= program.length){
            programPtr += 1;
        }
    } else {
        braces.push(programPtr);
    }
},
```

Running `node` quickly shows us that `93 != "]"` is `true`. So... we never end the loop. Instead, we continue to go off the end... running out of bounds of the program and calling `0xgg`.

Thus, we just need to take the string `'{"admin": "true", "guest": "false", "organization": "cyberlabs", "t": ""}'` (don't mind the `"t": ""`, I was already prepared to attack the original version where it would be with +3).

```python
raw = '{"admin": "true", "guest": "false", "organization": "cyberlabs", "t": ""}'

s = ''.join(chr(ord(c) - 2) for c in raw)
s = s[::-1]

transformString(s)

import requests

endpoint = 'http://34.76.206.46:10009'

res = requests.post(endpoint, data={'code': s})
print(res.text)
```

In the result, we see:

```
<div class="wrapper__description">
    jadeCTF{qU4Ck_qu4Ck!}
</div>
```

Success!
