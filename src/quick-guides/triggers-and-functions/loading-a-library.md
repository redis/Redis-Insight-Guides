When creating a new library we add the code to the command. The code need to contain a prologue where we define which engine is used, the name of the library and the minimum API version to use.

```redis Upload library
TFUNCTION LOAD // Load the following library
“#!js name=lib api_version=1.0\n redis.registerFunction(‘hello’, ()=> {return ‘Hello world’});” // The code of the library top upload
```

To view the uploaded library we can use the following command.

```redis View libraries
TFUNCTION LIST
```

Execute the function that was uploaded.

```redis Execute function
> FCALL // FCALL for running sync functions, FCALLASYNC for async
Lib.hello // the name of the library.function
0 // the number of keys given
```

We can give arguments when we are executing the command.
To update the library, add the replace argument.

```redis Replace the library
TFUNCTION LOAD REPLACE "#!js name=lib api_version=1.0\n redis.registerFunction('hello', (client, arg)=> {return ‘Hello ’ +  arg});"
```

Now run the command with an additional argument

```redis Execute function
FCALL // FCALL for running sync functions, FCALLASYNC for async
Lib.hello // the name of the library.function
0 // the number of keys given
John
```

The library can be deleted with the following command.

```redis Delete library
TFUNCTION DELETE lib
```