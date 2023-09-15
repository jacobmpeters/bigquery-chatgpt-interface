Objective: Use the ChatGPT and BigQuery APIs to explore a dataset using natural language queries. Address additional data dictionary mapping problems.

# Google BigQuery Chatbot

natural_language_query.py is a Python script that uses the OpenAI GPT-3.5 Turbo model to create SQL queries in Google BigQuery based on natural language queries. The script also handles errors and provides interactive communication with the user. Eventually, I plan to implement automatic mapping of concepts to concept IDs using a data dictionary.

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Configuration](#configuration)
- [Examples](#examples)

## Installation

1. Clone the repository:

   ```bash
   git clone https://github.com/your-username/your-repo.git
   cd your-repo
   ```

2. Install the Python packages:

```bash
pip install openai pandas google-cloud-bigquery python-dotenv
```

3. Set up your environment variables by creating a .env file and adding your OpenAI API key:
OPENAI_KEY=your_api_key_here

4. Run the script:
```bash
python your_script.py
```

## Usage
Provide a natural language query to the script, and it will generate a corresponding SQL query for BigQuery.

The script communicates with the OpenAI GPT-3.5 Turbo model to assist in formulating SQL queries.

If any errors occur during query execution in BigQuery, the script will attempt to correct the query based on the error message received.

The script provides formatted BigQuery results and communication logs for troubleshooting.

## Configuration
Configure the script by setting your OpenAI API key in the .env file as described in the Installation section.

## Examples
Here are some example scenarios:

### Example 1 (No Error Handling Required):

#### Inputs
```python
full_table_name = 'bigquery-public-data.usa_names.usa_1910_2013'
nat_lang_query = 'What are the top 5 baby names in 2012 that begin with the letter A?'
```

#### Natural Language Query:

What are the top 5 baby names in 2012 that begin with the letter A?

 
#### SQL From GPT:

```sql
SELECT name, number
FROM `bigquery-public-data.usa_names.usa_1910_2013`
WHERE year = 2012
    AND name LIKE 'A%'
ORDER BY number DESC
LIMIT 5;
```

#### BigQuery Results:

| name      |   number |
|:----------|---------:|
| Alexander |     2421 |
| Anthony   |     2417 |
| Andrew    |     2142 |
| Aiden     |     1994 |
| Angel     |     1929 |


#### GPT Messages:

```JSON
{
    "role": "system",
    "content": "Act as if you're a data scientist who uses exclusively GoogleSQL syntax in BigQuery. \nNote that in 2021, GoogleSQL was called Google Standard SQL.\n\nYou have a BigQuery table named bigquery-public-data:usa_names.usa_1910_2013 with the following schema:\n```[SchemaField('state', 'STRING', 'NULLABLE', None, '2-digit state code', (), None), SchemaField('gender', 'STRING', 'NULLABLE', None, 'Sex (M=male or F=female)', (), None), SchemaField('year', 'INTEGER', 'NULLABLE', None, '4-digit year of birth', (), None), SchemaField('name', 'STRING', 'NULLABLE', None, 'Given name of a person at birth', (), None), SchemaField('number', 'INTEGER', 'NULLABLE', None, 'Number of occurrences of the name', (), None)]```\n\nThe first rows look like this: \n```  state gender  year     name  number\n0    AL      F  1910    Sadie      40\n1    AL      F  1910     Mary     875\n2    AR      F  1910     Vera      39\n3    AR      F  1910    Marie      78\n4    AR      F  1910  Lucille      66```\n\nBased on this data, write a SQL query to answer my questions.\nReturn the SQL query ONLY so that it will be executable in BigQuery.\nDo not include any additional explanation.\nRemember that table names must be in the form of `project.dataset_id.table_id` in a GoogleSQL query.\n"
}
{
    "role": "user",
    "content": "What are the top 5 baby names in 2012 that begin with the letter A?"
}
{
    "role": "assistant",
    "content": "SELECT name, number\nFROM `bigquery-public-data.usa_names.usa_1910_2013`\nWHERE year = 2012\n    AND name LIKE 'A%'\nORDER BY number DESC\nLIMIT 5;"
}
```
