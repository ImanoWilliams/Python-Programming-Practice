# Python-Programming-Practice (Python 2.7)
1. In a file animal.py, define a new-style class Animal and use it as the base class for the definitions of classes Sheep and Cow (also in animal.py). Instances of Animal (instances of Sheep or Cow) as such have data attributes id (positive integer, identifier), weight (float, in pounds), gender (string, one of 'f', 'm', or 'n' (for ‘neuter’, a steer or wether)), and age (float, in years). Animal() is called with values for id, weight, gender, and age (in that order).
Instances of Sheep and Cow (and of any class derived from Animal) should implement a method requires(), which returns how many acres of pasture the instance requires. For Animal, make this essentially an abstract method (and Animal an abstract class) by having it raise a NotImplementedError with argument "Please Implement method requires" (and do nothing else). (For the more sophisticated approach to abstract classes, see the Python ABC (Abstract Base Classes) module.)
Implement special method __str__() in Animal but not in Sheep or Cow. When an instance of Animal (i.e., an instance of Cow or Sheep) is converted to a string, it returns a string of the form "<ClassName> <id>" where "ClassName> is “Sheep” of “Cow” and <id> is the value of the id attribute of self. You can find the class name by accessing the __class__ attribute of self then the __name__ attribute of the class.
Animal also has an info() instance method, which takes no arguments and prints a string of the form
"<ClassName> <id>", weight: <weight>, gender: <gender>, age: <age>
The first part of this string is just what is returned by the string conversion. In the rest, <…> indicates the actual value as thus described.
Sheep instances have as data attributes, besides those inherited from Animal, wool_prod (float, annual wool production, in pounds) and wormed (Boolean, whether self has been wormed, initialized to False). Sheep() is called with the values for id, weight, gender, age, and wool_prod (in that order) to initialize the data attributes; wormed is initialized to False. For initialization, call the __init__() method of Animal to do what is common. Where sh is a Sheep instance, sh.requires() returns 0.1 + self.weight * 10-3. And sh.info() calls the info() method of Animal and then outputs an additional line with the values of self.wool_prod and self.wormed. Sheep also has a worm() method to set the wormed attribute to True.
Cow instances have as data attributes, besides those inherited from Animal, milk_prod (float, annual milk production, in pounds) and tagged (Boolean, whether self has been tagged, initialized to False). Cow() is called with the values for id, weight, gender, age, and milk_prod (in that order) to initialize the data attributes; tagged is initialized to False. For initialization, call the __init__() method of Animal to do what is common. If something other than 'f' is specified for gender and a value greater than 0 is specified for milk_prod, raise an Exception with a string argument of the form
"Cow <id> must be female to produce milk."
Where cw is a Cow instance, cw.requires() returns 0.5 + self.weight * 1.25 * 10-3. And cw.info() calls the info() method of Animal and then outputs an addition line with the
COMP 895 Advanced Data Science & Cyberidentity Fall 2016 Assignment 2
2
values of self.milk_prod and self.tagged. Cow also has a tag() method to set the tagged attribute to True.
The following is code that you can put in animal.py to test your class definitions.
if __name__ == "__main__":
cow1 = Cow(62, 1189.0, 'f', 6.3, 18000)
cow1.info()
print
print "Requires " + str(cow1.requires()) + "\n"
cow1.tag()
cow1.info()
print "\n"
sheep1 = Sheep(43, 146.0, 'f', 5.5, 10.5)
sheep1.info()
print
print "Requires " + str(sheep1.requires()) + "\n"
sheep1.worm()
sheep1.info()
print "\n"
try:
cow1 = Cow(57, 1189.0, 'm', 6.3, 18000)
except Exception, detail:
print detail
The following is the output produced.
Cow 62, weight: 1189.0, gender: f, age: 6.3
tagged: False, milk per year: 18000
Requires 1.98625
Cow 62, weight: 1189.0, gender: f, age: 6.3
tagged: True, milk per year: 18000
Sheep 43, weight: 146.0, gender: f, age: 5.5
wormed: False, wool per year: 10.5
Requires 0.246
Sheep 43, weight: 146.0, gender: f, age: 5.5
wormed: True, wool per year: 10.5
Cow 57 must be female to produce milk.

In a file pasture.py, define a class Pasture with data attributes id (integer, an identifier), acres (float, the number of acres self occupies), and animals (a list of instances of Animal, the animals, instances of Cow or Sheep, currently grazing in the pasture). When Pasture() is called, it is passed values for id and acres; animals is initialized to the empty list (there are initially no animals in the pasture). When an instance of Pasture is converted to a string, it returns its id converted to a string. Pasture has a method print_animals(), called with no arguments, which prints all elements in self.animals as converted to strings (for each, its class name followed by its id). It also has a method total_requirements(), also called with no arguments. This returns the sum of the values returned by the requires() methods of all the elements in self.animals. (That is, it returns the number of acres needed to support all these animals.)
Method add_animals() is passed a list of instances of Animal (actually, instances of Sheep and instances of Cow) and adds them to self.animals as long as there are enough acres left for them to graze. To see whether this is the case, initialize total_req to what is returned by self’s total_requirements() method, and then add to it the values returned by the requires() methods of all the animals in the list of animals we are trying to add. If total_req exceeds the value of self’s acres attribute, then raise an Exception with a string of the following form as argument.
"Adding <animals> exceeds the carrying capacity of pasture <id>"
Here <id> is the id of self, and <animals> is a sequence of the string values, separated by spaces, of the elements in self.animals (e.g., "Sheep 51 Cow 32 Sheep 63").
Method remove_animals() is passed a list of the ids of animals to remove from self.animals. (Note that this method is passed a list of ids, not, like add_animals(), a list of instances of Animal.). The implementation is quite straightforward.
The following is code that you can put in pasture.py to test you class definition.
if __name__ == "__main__":
import animal as an
cow1 = an.Cow(62, 1250.0, 'm', 6.3, 0)
cow2 = an.Cow(71, 1150.0, 'f', 6.3, 18000)
sheep1 = an.Sheep(43, 150.0, 'f', 5.5, 10.5)
sheep2 = an.Sheep(47, 160.0, 'm', 4.5, 12.5)
sheep3 = an.Sheep(53, 140.0, 'f', 5.0, 10.0)
sheep4 = an.Sheep(51, 155.0, 'f', 6.5, 9.0)
pasture1 = Pasture(5, 5.0)
pasture1.add_animals([cow1, cow2, sheep1, sheep2, sheep3])
print "The animals in pasture " + str(pasture1) + " are"
pasture1.print_animals()
print
print "They require %.2f acres to graze." % pasture1.total_requirements()
try:
pasture1.add_animals([sheep4])
except Exception, detail:
print "\n" + str(detail)
pasture1.remove_animals([cow1.id, sheep1.id])
print "\nThe animals in pasture " + str(pasture1) + " are"
pasture1.print_animals()
print
