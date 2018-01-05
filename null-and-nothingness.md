# Null & Nothingness

What is nothingness? Is there nothingness only in outer space? Really, when we think of "nothing", isn’t it just the absence of something? OK, that’s too much philosophy…

nil is Ruby’s way of referring to "nothingness."

If you have three eggs, eat three eggs, then you might think you have "nothing", but in terms of eggs you have "0". Zero is something, it’s a number, and it’s not nothing.

If you’re working with words and have a string like "hello" then delete the "h", "e", "l"s, and "o" you might think you’d end up with nothing, but you really have "" which is an empty string. It’s still something.

nil is Ruby’s idea of nothingness. It’s usually encountered when you ask for something that doesn’t exist. When looking at arrays, for instance, we created a list with five elements then asked Ruby to give us the sixth element of that list. There is no sixth element, so Ruby gave us nil. It isn’t that there’s a blank in that sixth spot (""), it’s not a number 0, it’s nothingness – nil.

A large percentage of the errors you encounter while writing Ruby code will involve nil. You thought something was there, you tried to do something to it, and you can’t do something to nothing so Ruby raises an error.