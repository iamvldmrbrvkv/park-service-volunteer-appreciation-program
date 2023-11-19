# park-service-volunteer-appreciation-program
[Codecademy](https://www.codecademy.com/learn) [TypeScript](https://www.typescriptlang.org/) project.

## Park Service Volunteer Appreciation Program
The Park Service has just inherited two parks: Wolf Point Park and Raccoon Meadows. Each park has a volunteer program where volunteers help maintain the parks by cleaning campsites, planning educational events, and maintaining hiking trails.

The Park Service would like to combine their volunteers and introduce a volunteer appreciation program, where the top volunteers get a special edition park badge for their service. It’s your job to help complete a program that was partially written by a colleague to help the Park Service determine this season’s top volunteers.

You’ll need to take your colleague’s code, combine data from both park’s volunteer logs, then calculate which volunteers have the most hours. Get your bug spray and hiking boots and let’s type out this program!

## Tasks
### Get familiar with the code
1. Welcome to the Park Service. Let’s inspect some of the code that already exists so that we can get familiar with the data we’ll be working with:

- index.ts: Contains incomplete code that a colleague put together. At the top of their code, they’re importing data and types from a couple of files. Later on in their code, they made some incomplete functions, defined some types, and began on some of the logic we can use to calculate volunteer hours.
- tsconfig.json: Defines the rules for TypeScript to compile code into JavaScript.

It’s worthwhile to look through these files so that you’ll understand the structure of this project.

2. Take a look at two files that contain volunteer logs: raccoon-meadows-log.ts and wolf-point-log.ts. Both of these files contain a list of data and types for how that data is structured.

Notice that the two volunteer logs have similar data but it’s often in different formats. For instance, the id values in raccoon-meadows-log.ts are numbers while the id values in wolf-point-log.ts are strings.

The quickest way to see all the differences between the two logs is to compare the exported types from each file. Take a look at each file and become familiar with the data.

3. Navigate back to index.ts. Below the lines that import data and types from the volunteer logs, there are two types:

- CombinedActivity: This type is a union type of RaccoonMeadowsActivity and WolfPointActivity. CombinedActivity will allow all the keys and properties that RaccoonMeadowsActivity and WolfPointActivity share.
- Volunteers: This type defines what we would like our combined volunteer data to look like once we combine our volunteers into a single list.

By creating types first, you can get a clear idea of what changes you’ll need to make. Once you’ve familiarized yourself with these types, let’s move on to combining the volunteer logs.

### Combine the volunteers
4. Examine the combineVolunteers() function and its volunteers argument, which is an array of RaccoonMeadowsVolunteers and WolfPointVolunteers. We’d like to take the list of volunteers of different types and combine them to match the Volunteers type.

Inside the body of the function, start by calling .map() on the volunteers array and providing a callback function. The callback function’s parameter should be named volunteer.

5. Inside the .map() method’s callback, we’d like to make sure that all volunteers have an id of type number. Our goal is to make our combinedVolunteers match the Volunteers type.

Start by declaring a variable named id using let. Set the id variable’s value to volunteer.id.

6. Inside the .map() method’s callback, let’s transform the id‘s of type string into number types.

Start by writing a type guard that checks if id is a 'string' using the typeof operator.

7. If id is of type string, then turn the id into a number inside the type guard.

Since our ids of type string look like '400sg', for example, we can use the parseInt() function to transform the id into a number. You can accomplish this by setting id equal to parseInt(id, 10).

8. Below the type guard, return an object that matches the Volunteers type.

You should return an id, name, and activities properties inside the object. Make sure to assign the id you declared, since you conditionally changed its value with the type guard. You can get the other values from the volunteers parameter.

9. Finally, make sure to return the result of the volunteers.map() call you made at the beginning of the combineVolunteers() function.

10. Let’s view our combined list. At the bottom of index.ts, write a console.log() to print out combinedVolunteers.

Save your code. Then in the terminal, run tsc to compile your code, then node index.js to see the result. You should see a list of volunteers from each volunteer log print to the console.

### Calculate hours
11. Now that we have a combined list of volunteers, we need to calculate each volunteer’s verified hours.

Let’s take a look at the calculateHours() function.

- The calculateHours() function takes in a list of volunteers, which is a list of Volunteer typed items.
- Then .map() is called on volunteers to iterate through each volunteer and return a new array.
- Inside the callback of the .map() method, .forEach() is called on volunteer.activities. .forEach() is an array method that will execute a function for each item in the list.
- At the end of the .map() method’s call back, the method returns an object with a volunteer id, name, and hours.

Inside the .forEach() method’s callback, we need to do two things to get a volunteer’s total hours:

1. Make sure that the activity is verified. Volunteer hours must be verified by a park staff member to count toward a badge.
2. If their activity is verified, then we need to add their hours to the hours variable.

Once we have the hours from each volunteer, calculateHours() will return a list of all volunteers and their verified hours.

12. Inside the .forEach() method’s callback, we need to write code that will check if a volunteer’s hours are verified. Let’s accomplish this by writing a separate function.

Start by declaring a function above calculateHours() named isVerified().

isVerified() should take a parameter named verified. Since Raccoon Meadows Park verifies hours with the strings 'Yes' or 'No' and Wolf Point Park verifies hours with booleans, verified should be a union type of either string or boolean.

13. Inside isVerified(), we need to check if verified is a string.

Write a type guard using the typeof operator. Inside the type guard, return true if verified equals 'Yes', and false if verified equals 'No'.

14. Underneath the type guard, we know that verified must now be of type boolean.

After the type guard, return verified.

15. Now we can use our function to verify activities.

Inside calculateHours(), find the .forEach() method’s callback function. Inside the .forEach() method’s callback, write a conditional that checks if the activity is verified. We can do that by calling isVerified() and passing in activity.verified as the argument.

16. If the activity is verified, we need to add the activity’s number of hours to the hours variable.

Raccoon Meadows Park tracks hours with an hours property on each item in activities. Wolf Point Park, on the other hand, tracks hours with a time property on each item in activities. Let’s write another helper function to get the correct property.

Declare a function above calculateHours() named getHours(). getHours() should take an argument named activity. Since activity will be an item from activities from either park, set its type as CombinedActivity.

17. Inside getHours(), write a type guard using the in keyword to check if the 'hours' key exists on activity.

If it does, then return activity.hours. If it does not, use an else statement to return activity.time.

18. Look back at the calculateHours() function, where you wrote your conditional checking if an activity is verified with the isVerified() function.

Inside the conditional, set the hours variable equal to itself plus the activity’s number of hours. Call the getHours() function with activity as its argument to get back the correct number of hours.

19. calculateHours() is now complete. Let’s take it for a spin.

At the bottom of the program, declare a variable named result. Its value should be a call to calculateHours() with the combined list of combinedVolunteers as its argument. Then use console.log() to print result.

Save your code. Then use tsc and node index.js in the console to see the output. You should see a list of volunteers with their total number of hours.

### Wrap it up
20. We have a list of volunteers and their hours! Now let’s put them in order. To do so, we’ll need to use the .sort() method with a custom comparator function.

Below calculateHours(), declare a function named byHours().

byHours() should take two arguments: a and b. Inside the function return b.hours - a.hours. We will use this to sort the result items by their hours property.

21. Finally, let’s call the .sort() method on result.

In a console.log(), call .sort(byHours) on result.

Save your code. Then in the terminal, run tsc to compile your code and node index.js to see the top volunteers.

22. 🌲👏 The trees are clapping for you and the volunteers are now more energized than ever. Nice work on using type guards to combine, verify, sum, and sort the parks’ volunteers. The Park Service can now send special edition badges to their top volunteers.