# Full Stack Trivia API Backend

## Getting Started

### Installing Dependencies

#### Python 3.7

Follow instructions to install the latest version of python for your platform in the [python docs](https://docs.python.org/3/using/unix.html#getting-and-installing-the-latest-version-of-python)

#### Virtual Environment

We recommend working within a virtual environment whenever using Python for projects. This keeps your dependencies for each project separate and organized. Instructions for setting up a virtual environment for your platform can be found in the [python docs](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/)

#### PIP Dependencies

Once you have your virtual environment setup and running, install dependencies by navigating to the `/backend` directory and running:

```bash
pip install -r requirements.txt
```

This will install all of the required packages we selected within the `requirements.txt` file.

##### Key Dependencies

- [Flask](http://flask.pocoo.org/) is a lightweight backend microservices framework. Flask is required to handle requests and responses.

- [SQLAlchemy](https://www.sqlalchemy.org/) is the Python SQL toolkit and ORM we'll use handle the lightweight sqlite database. You'll primarily work in app.py and can reference models.py.

- [Flask-CORS](https://flask-cors.readthedocs.io/en/latest/#) is the extension we'll use to handle cross origin requests from our frontend server.

## Database Setup

With Postgres running, restore a database using the trivia.psql file provided. From the backend folder in terminal run:

```bash
psql trivia < trivia.psql
```

## Running the server

From within the `backend` directory first ensure you are working using your created virtual environment.

To run the server, execute:

```bash
export FLASK_APP=flaskr
export FLASK_ENV=development
flask run
```

Setting the `FLASK_ENV` variable to `development` will detect file changes and restart the server automatically.

Setting the `FLASK_APP` variable to `flaskr` directs flask to use the `flaskr` directory and the `__init__.py` file to find the application.

## Testing

To run the tests, run

```
dropdb trivia_test
createdb trivia_test
psql trivia_test < trivia.psql
python test_flaskr.py
```

## API Reference

### Endpoints

- [GET '/categories'](#get-categories)
- [GET '/questions'](#get-questions)
- [POST '/questions/search'](#post-questionssearch)
- [DELETE '/questions/<int:question_id>'](#delete-questionsintquestion_id)
- [POST '/questions'](#post-questions)
- [GET '/category/<int:category_id>/questions'](#get-categoryintcategory_idquestions)
- [POST '/quizzes'](#post-quizzes)

#### GET '/categories'

- General
  - Fetches a dictionary of categories in which the keys are the ids and the value is the corresponding string of the category
  - Request Arguments: None
  - Returns: An object with a single key, categories, that contains a object of id: category_string key:value pairs.
- Sample: `curl http://localhost:5000/categories`

```
{
  'categories': {
    '1' : "Science",
    '2' : "Art",
    '3' : "Geography",
    '4' : "History",
    '5' : "Entertainment",
    '6' : "Sports"
  }
}
```

#### Get '/questions'

- General
  - Fetches a paginated list of questions, answers, their difficulty levels and categories.
  - Request Arguments:
    - 'page': query param identifying the start and end of the list of questions to return
    - 'current_category': (optional) query param identifying how to filter down the questions by category
  - Returns: An object with a key, questions, containing questions, along with a count of all questions, a list of categories and the current category.
- Sample: `curl http://localhost:5000/questions?page=1&current_category=4`

```
{
  questions: [
    {
      'id' : 5,
      'question' : "Whose biography is entitled 'I Know Why the Caged Bird Sings'?",
      'answer': "Maya Angelou",
      'difficulty' : 2,
      'category': 4,
    },
    ...
  ],
  total_questions: 19,
  categories: {
    '1': "Science",
    '2': 'Art',
    '3': 'Geography,
    '4': 'History',
    '5' : "Entertainment",
    '6' : "Sports"
  },
  current_category: 4,
}
```

#### Post '/questions/search'

- General
  - Searches for matching questions using a query
  - Request arguments:
    - body - with a single key, search, that contains string query to match, case-insensitive
  - Returns - list of questions that match the searched text
- Sample: `curl http://localhost:5000/questions/search -X POST -H "Content-Type: application/json" - d '{ search: 'caged bird' }'`

```
{
  questions: [
    {
      'id' : 5,
      'question' : "Whose biography is entitled 'I Know Why the Caged Bird Sings'?",
      'answer': "Maya Angelou",
      'difficulty' : 2,
      'category': 4,
    }
  ],
  total_questions: 19,
  categories: {
    '1': "Science",
    '2': 'Art',
    '3': 'Geography,
    '4': 'History',
    '5' : "Entertainment",
    '6' : "Sports"
  },
  current_category: null,
}
```

#### Delete '/questions/<int:question_id>'

- General
  - Deletes a question
  - Request Arguments:
    - question_id: variable which identifies the specific question
  - Returns: An object with a key, success, which is true
  - Sample: `curl http://localhost:5000/questions/42 -X DELETE`

```
{ success: true }
```

#### Post '/questions'

- General
  - Creates a new question with the passed in form json data
  - Request Arguments:
    - body: an object containing question, answer, difficulty and category
  - Returns: An object with a single key, success, signifying whether the question was saved
- Sample: `curl http://localhost:5000/categories -X POST -H "Content-Type: application/json" - d '{ question: 'foo?', answer: 'bar', difficulty: 10, category: 5 }'`

```
{ success: true }
```

#### Get '/category/<int:category_id>/questions'

- General
  - Gets all questions associated with a particular category
  - Request Arguments:
    - category_id: variable which identifies the category to filter the questions
- Sample: `curl http://localhost:5000/categories/4/questions`

```
{
  questions: [
    {
      'id' : 5,
      'question' : "Whose biography is entitled 'I Know Why the Caged Bird Sings'?",
      'answer': "Maya Angelou",
      'difficulty' : 2,
      'category': 4,
    },
    ...
  ],
  total_questions: 19,
  current_category: null,
}
```

#### Post '/quizzes'

- General
  - Accepts a list of questions and a category, and returns a fresh question
  - Request Arguments:
    - previous_questions - list of old questions
    - quiz_category - (optional) filter while selecting a new question
- Sample: `curl http://localhost:5000/quizzes -X POST -H "Content-Type: application/json" - d '{ previous_questions: [6, 4], quiz_category: 5 }'`

```
{
  question: {
    'id' : 5,
    'question' : "What movie earned Tom Hanks his third straight Oscar nomination, in 1996?",
    'answer': "Apollo 13",
    'difficulty' : 4,
    'category': 5,
  }
}
```
