# SparkCloud

The SparkCloud.jl package allows you to connect your Julia programs to any SparkCore device (or devices) via the SparkCloud API.

## Getting started ##

To get connected, all you need to know is the unique device id of your SparkCore (e.g. 0123456789abcdef01234567) which can be found on https://www.spark.io/build and your username and password.

    using SparkCloud
    core = SparkCore("0123456789abcdef01234567",
                     "joe@example.com",
                     "supersecretpassword")

This will login to the SparkCloud and get an access token for your account. But if you already have an access token you can use it directy:

    core = SparkCore("0123456789abcdef01234567",
                     "1234123412341234123412341234123412341234")

## Calling functions ##

Now you can start sending commands to your core. Currently only calling functions is supported:

    functioncall(core, "brew")

or with parameters:

    functioncall(core, "announce", "hello world") 
    functioncall(core, "goto", 100, 400) 

Obviously you can only call the functions that are defined in your firmware.

## Tinker ##

The standard Tinker firmware that ships on the SparkCore defines four functions that are made available in Julia in the `SparkCloud.Tinker` module: 

    Tinker.digitalwrite(core, pin, value)
    Tinker.digitalread(core, pin)
    Tinker.analogwrite(core, pin, value)
    Tinker.analogread(core, pin)

So to light the built in LED do:

    Tinker.digitalwrite(core, "D7", "HIGH")

## @sparkfunction ##

The functions in `Tinker` are defined with the help of the handy `@sparkfunction` macro which can be used in your own code to declare Julia a function that maps to a function in your SparkCore's firmware:

    module MyAmazingMachine
      @sparkfunction goto x y
    end
    MyAmazingMachine.goto(100, 400)

