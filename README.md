# Iterators, blocks and CRUD with hashes & arrays in Ruby



***SUMMARY***

 - [Iterators](#Iterators)
     - [For Loop with indexes](#For-loop-with-indexes)
     - [While loop with indexes](#While-loop-with-indexes)
     - [Each](#Each)
     - [Each With Index](#Each-with-index)
     - [Difference between ```each_with_index```  and ```each_index```](#Difference-between-```each_with_index```-and-```each_index```)
     - [Map](#map)
     - [Select](#select)
     - [Reject](#reject)
     - [Reduce](#reduce)
 - [Blocks](#Blocks)
 - [Yield](#Yield)
     - [Sum an array with yield](#Sum-an-array-with-yield)
     - [Creating an html tag with yield](#Creating-an-html-tag-with-yield)
 - [CRUD with Arrays](#CRUD-with-Arrays)
 - [CRUD with Hashes](#CRUD-with-Hashes)

 

 # Iterators
 
 Iterators (or Enumerators) are methods that help us return all the elements of a collection or group of data. 

 In Ruby, arrays and hashes can be termed as **collections**

 To 'iterate' means to do the same thing many times. We will see below different methods that help us iterate through our collections. 

 For more details and methods for different iterators, please check the ruby-doc of the Enumerable module :point_right: : https://ruby-doc.org/core-3.0.2/Enumerable.html

 ## For loop with indexes 

 ```ruby
destinations = ['Ha long bay', 'Amazon rainforest', 'the pyramids of Giza', 'Taj Mahal']

for index in (0...destinations.size)
    puts "#{index + 1 } - #{destinations[index]}" 
end
 ```

## While loop with indexes 

```ruby 
destinations = ['Ha long bay', 'Amazon rainforest', 'the pyramids of Giza', 'Taj Mahal']

counter = 0 
while  counter < destinations.size
    puts destinations[counter]
    counter += 1
end
```

 ## Each 


```ruby

destinations = ['Ha long bay', 'Amazon rainforest', 'the pyramids of Giza', 'Taj Mahal']

destinations.each {|place| puts place }
```

or using a multine block 

```ruby

destinations = ['Ha long bay', 'Amazon rainforest', 'the pyramids of Giza', 'Taj Mahal']

destinations.each do |place|
    puts place 
end
```


## Each with index 

```ruby

destinations = ['Ha long bay', 'Amazon rainforest', 'the pyramids of Giza', 'Taj Mahal']

destinations.each_with_index {|value,index| puts destinations[index]}

```


## Difference between ```each_with_index```  and ```each_index``` 

Not to confuse one another, ```each_with_index``` can extract both the element and indexes, while ```each_index``` only get us the index of our elements

**each_with_index** 
```ruby
colors = ['red', 'green', 'blue']
colors.each_with_index do |item, index|
	p "#{index}:#{item}" 
end
```
**each_index**
```ruby
colors = ['red', 'green', 'blue']
colors.each_index do |index|
	p "#{index}:#{colors[index]}" 
end
```


## map 
:warning: Difference between ```each``` and ```map```, map transforms the data in your collection by creating a new array with the new values, while ```each``` simply iterate without having to change anything outside its block.

```ruby 
week_end = ['Saturday','Sunday']

upper_case_wk = week_end.map {|e| e.upcase}

p upper_case_wk
# => ["SATURDAY", "SUNDAY"]
```

## select
Select help us iterate through a collection and filter the values according to our expression inside the block, it returns an array.

```ruby
numbers = [1,2,3,4,5]
numbers.select {|e| e%2 == 0}
#=> [2, 4]
#We are filtering even numbers
```

## reject 
Reject is the opposite of select, instead of filtering the values, it simply rejects any given expression or conditional inside our block, it returns an array as well.

```ruby
numbers = [1,2,3,4,5]
numbers.reject {|e| e%2 == 0}
#=>[1, 3, 5]
#It rejects even numbers and give us the odd ones only.
```


## reduce 
Reduce help us redue an array into a single value. For each element of the array, the block gets two arguments an ```accumulator``` and ```current element``` . 

One simple example is to sum an array with reduce

```ruby
numbers = [1,2,3,4,5]
numbers.reduce {|acc,element| acc + element}
#=> => 15
```

There is also ```inject``` which is just the alias of reduce, they both do the same thing.


# Blocks 

What are blocks ? They are anonymous functions that can be passed on methods ! 

Syntax of a block can be written either with ```do end``` or ```{}``` with ```|``` two pipe characters that holds one argument or many.

*one line block*
```ruby
    [1,2,3].each {|num| p num if num.odd?}
```

*multiline block*
```ruby
    [1,2,3].each do |num|
       p num if num.odd?
     end
```

 # Yield

 Yield is a built-in method in Ruby that helps us call a block and run the code inside. 

It can pass any number of arguments to the block from the yield.

Let's make this clear, Yield is really NOT commonly used by Ruby developers, the idea is to have a method which can be called in two different ways,
with two different behaviours, either with a block or not.

Most of the Rubyist these days are simply creating regular methods with parameters and avoiding too much behaviours in calling a method. 

However, understanding YIELD is essential in order to understand a code block in Ruby On Rails that involves ```yield```, we will furthermore discuss about this during your Ruby On Rails lectures ! 

Let's start with simple examples : 

```ruby 
def yield_ex
    puts "We are inside our method"
    yield 
    puts "Again we are back in the method"
    yield 
end 

yield_ex {puts "This is the block of the code"}
```

```console
We are inside our method
This is the block of the code
Again we are back in the method
This is the block of the code

```


Let's study a problem in this code : 

```ruby
start = Time.now
puts "Task 1 "
sleep(2)
puts "Task 1 DONE !"
stop = Time.now 
puts "Time elapsed : #{(stop - start).to_i}s"


start = Time.now
puts "Task 2 "
sleep(2)
puts "Task 2 DONE !"
stop = Time.now 
puts "Time elapsed : #{(stop - start).to_i}s"

start = Time.now
puts "Task 3 "
sleep(2)
puts "Task 3 DONE !"
stop = Time.now 
puts "Time elapsed : #{(stop - start).to_i}s"

```

Yield could be helpful in this case to make it less redundant: 


```ruby

def counter
    start = Time.now
    yield
    stop = Time.now
    puts "Time elapsed : #{(stop - start).to_i}s"

end

counter do 
    puts "Task 1 "
    sleep(5)
    puts "Task 1 Done"
end

counter do 
    puts "Task 2 "
    sleep(5)
    puts "Task 2 Done"
end

```

Example I

```ruby 

    def recipe_list
        yield "meat"
        yield "tomatoe"
        yield "curry"
    end 

    recipe_list {|list| p "We are making #{list}"}

```

```console
"We are making meat"
"We are making tomatoe"
"We are making curry"
```


Example II 


```ruby

def yield_iteration
    yield 5
    puts "you are inside the method Yield iteration"
    yield 100 
    puts "You are back in the method yield"
end

yield_iteration {|i| puts "this is the iteration number #{i}" }
```

```console
this is the iteration number 5
you are inside the method Yield iteration
this is the iteration number 100
You are back in the method yield
```

Example III 

```ruby
def yield_name
    yield "Kawtar"
    puts "you are inside the method Yield name"
    yield "Anouar"
    puts "You are back in the method yield"
end

yield_name {|i| puts "this is the name from the block => #{i}" }

```

```console
this is the name from the block => Kawtar
you are inside the method Yield name
this is the name from the block => Anouar
You are back in the method yield
```


## Sum an array with yield 

```ruby
def sum_array_with_block(arr)
    if block_given?
        counter = 0
        while counter < arr.size 
            yield(arr[counter])
            counter += 1 
        end
    else 
        arr.sum
    end
end

sum = 0
sum_array_with_block([1,2,3,4,5]) { |el| sum += el}
p sum 
```

## Creating an html tag with yield 

```ruby
def tag(tag_name, attributes = nil)
  attr_name, attr_value = attributes unless attributes.nil?  #Destructuring
  open_tag = attributes.nil? ? tag_name : "#{tag_name} #{attr_name}=\"#{attr_value}\""
  "<#{open_tag}>#{yield}</#{tag_name}>"
end
```

 

## CRUD with Arrays 

```ruby
    arr = []

    #Create
    arr.push('batch 775')
    arr << ('Casablanca')
    p arr
    #=> ["batch 775", "Casablanca"]

    #Read 
    p arr[0]
    #=> "batch 775"

    #Update 
    arr[1] = 'Morocco'
    #=> "Morocco"

    #Delete 
    arr.delete('batch 775')
    p arr 
    #=> ["Casablanca"]
    arr.delete_at(0)
    #> []
```

## CRUD with Hashes

```ruby
    obj = {}

    #Create
    obj['batch'] = '775'
    obj['city'] = 'Casablanca'

    p obj

    #=> {"batch"=>"775", "city"=>"Casablanca"}

    #Read 
    p obj['city']
    #=>"Casablanca"

    #Update 

    obj['batch'] = '775 ROCKS!' 
    p obj

    #=> {"batch"=>"775 ROCKS!", "city"=>"Casablanca"}
    #Delete 

    obj.delete('batch')
    p obj
    #=> {"city"=>"Casablanca"}
```