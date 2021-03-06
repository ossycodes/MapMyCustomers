
# Solution
1. I start with seeding all data into a mongoose database on the start of the application, while seeding I create each store latitude and longtitude in a mongodb Geocode format.

2. Next, in my application controller I make use of [node-geocoder](https://www.npmjs.com/package/node-geocoder) library to convert zipcodes and addresses to Geocodable latitude and longitude using whatever query parameter is passed

3. I get the `latitude` and `longitude` from step 2 above, then use mongodb's  [$near](https://www.mongodb.com/blog/post/geospatial-performance-improvements-in-mongodb-3-2) method to fetch from my database the closest stores to these coordinates, I return the closest one.

4. Distance between `latitude` and `longitude` of step 2 and the stores coordinates is calculated in miles, using `calcDistance` function in `helpers/utils` <br> This is calculated usng the [Havesiner Formula](https://en.wikipedia.org/wiki/Haversine_formula) for Geometric distance

NB: I also created another method without the use of Mongodb. This method reads from the CSV file, and using javascript's [Reduce](), I am able to calculate the distance for each store (using the havesine formula), and return the store with the smallest distance to the address/zipcode

The reason this is not my main function is, mongodb's `near` function is much faster.

Observation: The pure javascript function beats the Mongodb function for zipcodes or addresses present in the top 100 of the store's list, after that it takes more time.

# Run
To run the app, 
1. Have `docker` and `docker-compose` installed
2. run ``` docker-compose up --build ```
3. to run tests, while the application is running, run ``` npm run test ```
# Coding challenge

In this folder there is store-locations.csv

This is a tabular dataset of the locations of every store of a major national retail chain.

# Deliverables

Write a simple server that can query the dataset and find the nearest store to a provided address or zip code

```
Find Store
  Your server will locate the nearest store (as the crow flies) from
  store-locations.csv, return the matching store address, as well as
  the distance to that store in JSON format

Usage:
  {server}/closest?zip=<zip>
  {server}/closest?address=<address>
  {server}/closest?zip=<zip>&units=<(mi|km)>

Options:
  ?zip=<zip>            Find nearest store to this zip code. If there are multiple best-matches, return the first.
  ?address=<address>  Find nearest store to this address. If there are multiple best-matches, return the first.
  ?units=(mi|km)        Display units in miles or kilometers [default: mi]

Note:
  addresses should be encoded for the URI
```

Additionally:

- Please write up a paragraph or two about how your solution works, any assumptions or caveats, and put it in a readme file.
- Your solution should be well-tested in the testing framework of your choice. Commit the test suite to your repo.
- The output format is not rigidly specified. Use your judgement for json formats.

Send a github link to the final project.

# Notes

Please complete this challenge using Node.js and focus on the problem itself (rather than framework/scaffolding). Please make sure it's reasonably easy to run your code and there are clear instructions for doing so.

You will need to use an external geocoding service. However please implement the distance calculation in your own code. To the extent you need any algorithms, I'm not expecting you to invent anything from scratch, so use Google & external libraries judiciously, and cite/document appropriately.

You can add polish or extra features if you'd like, but remember that software is about tradeoffs and *by far the most important thing is delivering working, practical software that solves the problem of finding the closest store location*. The goal is not to take up a bunch of your time, but see you solve a problem that looks very much like the type of work we do all the time.

There are a ton of different ways to skin this cat -- be smart, be practical, thanks, and good luck!
