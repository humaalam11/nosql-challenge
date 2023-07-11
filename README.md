# nosql-challenge
**Assignment 12**

**GOAL**
Evaluate the restaurant food ratings using MongoDB, NoSQL, database. 
- Import '.json' datafile into MongoDB
- Preform ETL functions, Extract, Transform and Load
- Transform data/ clean data

**PART 1: Database and Jupyter Notebook**
  1) Import json File
  2) Name datbase "uk_food"
  3) list the collections in "uk_food"
  4) Choose "establishments" and assign a variable to prep the collection to be used

**PART 2:  Update the Uploaded Database**
  1) Add in new information to the database for a new "halal" restaurant in Greenwich using MongoDB
  2) Data has a lot of files, this is an example of one looks like, and the fields it has:

  ![image](https://github.com/humaalam11/nosql-challenge/assets/130116747/19277ece-2786-43b3-82a5-53bd43d08739)
  
  3) Update the informatiion of the new restaurant. Use update_one() or update_many() function
  4) Change datatype:
     ex: db.establishments.update_many(
     {},
    [
        {
            "$set": {
                "geocode.longitude": { "$toDouble": "$geocode.longitude" },
                "geocode.latitude": { "$toDouble": "$geocode.latitude" }
            }
        }
    ]
)

**PART 3: Analysis**
  1) Find and change database
  2) Use query, sort and filter variables to split specific parameters
**ex:**
 # Search within 0.01 degree on either side of the latitude and longitude.
  # Rating value must equal 5
  # Sort by hygiene score

  degree_search = 0.01
  latitude = 51.10
  longitude = 1.18
  
  # Defining Latitude and Longitude parameters:
  
  latitude_max = latitude + degree_search
  latitude_min = latitude - degree_search
  
  longitude_max = longitude + degree_search
  longitude_min = longitude - degree_search
  
  # Creating query and sort parameters
  
  query = {'RatingValue': 5, 
           'geocode.latitude': {'$gte': latitude_min, '$lte': latitude_max},
           'geocode.longitude': {'$gte': longitude_min, '$lte': longitude_max}
          }
  sort =  [("scores.Hygiene", -1)]
  
  limit = 5
    **Rating value must equal 5
    Sort by hygiene score**
  
    degree_search = 0.01
    latitude = 51.10
    longitude = 1.18
  
    **Defining Latitude and Longitude parameters:**
  
    latitude_max = latitude + degree_search
    latitude_min = latitude - degree_search
  
    longitude_max = longitude + degree_search
    longitude_min = longitude - degree_search
  
   ** Creating query and sort parameters**
  
    query = {'RatingValue': 5, 
           'geocode.latitude': {'$gte': latitude_min, '$lte': latitude_max},
           'geocode.longitude': {'$gte': longitude_min, '$lte': longitude_max}
          }
    sort =  [("scores.Hygiene", -1)]
  
    limit = 5"
    
**3) Create pipiline:**
Ex: create match, groups and sort variables then plug into a 'pipeline' variable to apply multiple filters to the database and output the results.
  1. Matches establishments with a hygiene score of 0:
  
  match_query = {'$match': {'scores.Hygiene': 0}}
  
   2. Groups the matches by Local Authority
  group_query = {'$group': {'_id': "$LocalAuthorityName", 'count': { '$sum': 1 }}}
  
   3. Sorts the matches from highest to lowest:
  sort_values = {'$sort': { 'count': -1 }}
  
   Putting the pipeline together:
  pipeline = [match_query, group_query, sort_values]
  
  Running the pipeline with the "aggregate" function:
  results = list(establishments.aggregate(pipeline))
  
  Print the number of documents in the result:
  count = establishments.count_documents(query)
  print("Number of classifications in result: ", count)
  
  Print the first 10 results
  pprint(results[0:10])

4) Take the output, and display it as a table/ database using the list function
