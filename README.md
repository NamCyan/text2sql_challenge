# Text2SQL Challenge

Challenge: Build an AI model to generate SQL query from given user requirement and context.

## Dataset and Evaluation

### Dataset

You can get the train and test set in [here](./dataset)

- 105,851 records partitioned into 100,000 train and 5,851 test records
- Database context, including table and view create statements
- Natural language explanations of what the SQL query is doing

```
|-- id: integer                            Data record id
|-- domain: string                         Domain/vertical (e.g., aerospace)
|-- domain_description: string             Domain/vertical data description
|-- sql_complexity: string                 SQL code complexity label
|-- sql_complexity_description: string     SQL code complexity description
|-- sql_task_type: string                  Type of SQL task being performed
|-- sal_task_type_description: string      Description of the SQL task type
|-- sql_prompt: string                     Natural language SQL prompt
|-- sql_context: string                    Context for the SQL prompt
|-- sql: string                            SQL corresponding to the SQL prompt
|-- sql_explanation: string                Explanation of the SQL query
```

*Note: The output evaluation will use the `sql` attribute to calculate BLEU score*

*Examples:*

```json
{
  "id": 39325,
  "domain": "public health",
  "domain_description": "Community health statistics, infectious disease tracking data, healthcare access metrics, and public health policy analysis.",
  "sql_complexity": "aggregation",
  "sql_complexity_description": "aggregation functions (COUNT, SUM, AVG, MIN, MAX, etc.), and HAVING clause",
  "sql_task_type": "analytics and reporting",
  "sql_task_type_description": "generating reports, dashboards, and analytical insights",
  "sql_prompt": "What is the total number of hospital beds in each state?",
  "sql_context": "CREATE TABLE Beds (State VARCHAR(50), Beds INT); INSERT INTO Beds (State, Beds) VALUES ('California', 100000), ('Texas', 85000), ('New York', 70000);",
  "sql": "SELECT State, SUM(Beds) FROM Beds GROUP BY State;",
  "sql_explanation": "This query calculates the total number of hospital beds in each state in the Beds table. It does this by using the SUM function on the Beds column and grouping the results by the State column."
}
```

### Evaluation

BLEU-score: Evaluate the alignment between a machine's output and that of a human.

How to use? See [here](https://huggingface.co/spaces/evaluate-metric/bleu) for more details.

```python
# pip install evaluate
>>> predictions = ["SELECT * FROM tbo.CUSTOMERS", "SELECT TenSV, MSSV FROM QLDT"]
>>> references = [
...     ["SELECT * FROM CUSTOMERS"],
...     ["SELECT * FROM QLDT"]
... ]
>>> bleu = evaluate.load("bleu")
>>> results = bleu.compute(predictions=predictions, references=references)
>>> print(results)
```
_Note:_ The score obtained using the test set is not your final score; you will have a final evaluation on the last day of the topic. Focus on ensuring your model generalizes well.

## Submission

Submit a `json` file named `results.json` following the format provided below:

```jsonlines
{"id": 1, "answer": "SELECT * FROM tbo.CUSTOMERS"}
{"id": 2, "answer": "SELECT * QLDT"}
...
```

**!!!IMPORTANT**: You free to use the provided training data to build the model or any external source (**except testing data** of course). Your submission needs to at least surpass the baseline score.

*Please upload the submission to the assigned folder to each team. There is no restriction on the number of submissions. I will continuously update the Leaderboard based on your most recent submissions.*

If you have multiple submission versions. Please index the file name as: `results_{id}.json`


## Leaderboard

Your submission score will be shown in the leaderboard below. The highest score is in bold.

|GroupID|Score|Best score|
|:---|:---:|:---:|
|Group 1| 53.83 | 53.83 |
|Group 2| 54.21 | 54.21 |
|Group 3| 67.11 | 67.11 |
|Group 4| 62.23 | 62.23 |
|Group 5| 56.86| 56.86 |
|Group 6| 56.53 | 56.53 |
|Group 7| 65.32| 65.32 |
|Group 8| 73.69| **73.69** |
|Group 9| 59.96 | 59.96 |
|Baseline|55.23| |

## Deadline: 23h59 19/03/2025
