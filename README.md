# Preparing Crowdfunding analysis through ETL method

---

## Purpose of Analysis

This analysis uses ETL methodology (Extract, Transform, Load) to transform unstructured data into a structured form which is required for meaningful data analysis. The process of conducting the aforementioned is as follows;

1. Extracting Data from unstructured csv file
2. Transforming and cleaning the data using the dictionaries method
3. Creating an ERD and Schema to upload output into a relational database
4. Conduct SQL analysis on data

The above sequence is briefly explained in the summary points below;

### Extracting Data from unstructured csv file

The following steps were conducted to extract unstructured data into Python, Pandas and Jupyter Notebook;

* Import the unstructured csv file into Jupyter Notebook using `pd.read_csv`
* Convert the csv file into a dataframe
* Iterate through the dataframe and convert each row to a dictionary
* Iterate through each dictionary and get values through list comprehension and convert to list of values
* Convert the list values into a new dataframe with the columns separated
* Export the dataframe into a new csv file

### Transforming and cleaning the data using the dictionaries method

The following steps were conducted to clean semi-structured data using Python, Pandas and Jupyter Notebook;

(Note the cleaning steps are as per specified and required in this analysis)

* The analysis required to review datatypes using `.dtypes()` method and convert datatypes to appropriate formats where required
* The analysis required splitting columns using the `str.split` method
* The analysis required to drop unwanted columns using the `.column()` method
* The analysis required to reorder columns as per analysis requirement

### Creating an ERD and Schema to upload output into a relational database

The following steps were conducted to develop an Entity Relationship Diagram (table schema) to upload a table into a relational database;

* Using QuickDBD an ERD (Entity Relationship Diagram) was created specifying the table name, its datatypes, primary key and foreign key
* The above ERD enabled us to structure a query to create the backers table on our relational database
* Post which data was added to the table for further analysis

### Conduct SQL analysis on data

Once we have completed the ETL (Extract, Transform and Load) procedures in cleaning our data and uploading into a database, we looked at conducting in-depth analysis in providing insight to some key management questions;

1. What is the number of backers for each live cf_id campaign?

The following query was constructed to answer the above management question;

```
SELECT COUNT (ba.backer_id), ca.cf_id
FROM backers AS ba
INNER JOIN campaign AS ca
ON (ba.cf_id = ca.cf_id)
WHERE (ca.outcome = 'live')
GROUP BY ca.cf_id
ORDER BY COUNT DESC;
```

2. Develop a list of campaigners by their remaining goal amount

The following query was constructed to answer the above management question;

```
SELECT co.first_name, 
	co.last_name, 
	co.email,
	(ca.goal - ca.pledged) AS "Remaining Goal Amount"
INTO email_contacts_remaining_goal_amount
FROM contacts AS co
JOIN campaign AS ca
ON co.contact_id = ca.contact_id
WHERE ca.outcome = 'live'
ORDER BY "Remaining Goal Amount" DESC;
```

3. Develop a list of backers by the remaining goal amount of the campaign they support

The following query was constructed to answer the above management question;

```
SELECT ba.email,
	ba.first_name,
	ba.last_name,
	ca.cf_id,
	ca.company_name,
	ca.description,
	ca.end_date,
	(ca.goal - ca.pledged) AS "Left of Goal"
INTO email_backers_remaining_goal_amount
FROM backers as ba
JOIN campaign AS ca
ON ba.cf_id = ca.cf_id
ORDER BY "email" DESC;
```