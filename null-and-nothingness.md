# Null & Nothingness

What is nothingness? Is there nothingness only in outer space? If a tree falls in the forest and nobody is there to listen, does it make a sound? Starting to see the point? Does nothing really mean nothing? To be or not to be? Ok, I think we are going in a philosophical tangent, so let's get back to our geekiness:

`null` is Java's way to refer to "nothingness.", something that does not exist and has no value.  In CFML, we also use `null` even though the `null` keyword is yet to be introduced into the language.  

## Usage

We can use the `isNull()` method in CFML to evaulate for nothingness and we can even create a `null` value with a `javaCast( "null", "" )` function call or in Lucee you can use the `nullValue()` function as well.  However, please note that the `null` keyword is coming to CFML in the near future.

## In Practice

If you have three eggs, eat three eggs, then you might think you have "nothing", but in terms of eggs you have "0". Zero is something, it’s a number, and it’s not nothing.

If you’re working with words and have a string like "hello" then delete the "h", "e", "l"s, and "o" you might think you’d end up with nothing, but you really have "" which is an empty string. It’s still something.

Null in CFML is usually encountered when you ask for something that doesn’t exist. When looking at arrays, for instance, we created a list with five elements then asked CFML to give us the sixth element of that list. There is no sixth element, so CFML gave us null. It isn’t that there’s a blank in that sixth spot (""), it’s not a number 0, it’s nothingness – null.

**Examples**

```java

function getData( filter ){

    if( isNull( arguments.filter ) ){
      // then do this
    } else {
      // use the filter
    }

}

function returnsNull(){
  if( key.exists( "invalid" ) ){
    return key[ "invalid" ];
  }
}

results = returnsNull();

writeOutput( isNull( results ) );
```

Also note that if a function returns **nothing** it will be the same as returning `null`.
