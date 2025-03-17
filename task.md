# Improve API

## List

This section lists topics and sub-topics where the current student has misconceptions.

### Requirements

- Sub-topics are grouped by their parent topic.
- Each sub-topic will have a number of misconceptions (previously answered questions that the student got wrong).
- Sub-topics with no misconceptions will not be shown in this list.

### Request/Route

**Assumptions:**  
- A DB with questions, a foreign key linking the students to the questions, and a link table of student answers which tracks the student's response and whether or not they answered the question correctly.

**Route:** /improve/{student_id:int}/misconceptions  
**Request:** N/A - id route parameter is sufficient  
**Method:** GET

**Response:**
```json
{
  "topics": 
  [
    {
      "topicId": 1,
      "name": "string",
      "subTopics": 
      [
        {
          "topicId": 2,
          "parentTopicId": 1,
          "name": "string",
          "misconceptions": 
          [
            {
              "questionId": 1,
              "questionImageUrl": "string",
              "options": ["A", "B", "C", "D"],
              "correctAnswer": "A",
              "explanation": "string"
            },
            ...
          ]
        },
        ...
      ]
    }
  ]
}
```

**Other considerations:**
- Questions themselves (i.e. the images) could change in the response depending on how/where they are stored. If they are accessible via a url resource such as a cloud provider filestore (e.g. Amazon S3), a URL would be sufficient.
  - An alternative approach could be Base64 encoded string to return the image bytes in a JSON payload, however that increases the payload size considerably.
- The list of misconceptions could either return the full question (as above), or just the question ID to be used via a separate endpoint call for the View page.
  - Benefits of the above approach would be fewer API calls on the front end, and allows for the question data to be immediately available in one call.
  - Potential drawbacks are larger payloads and redundant data being returned if a question appears under multiple parent topics.
  - By going with just question ID, the system would benefit from smaller payloads, reduction of redundancy, and flexibility in when the data specific to a question is returned. However, this approach could increase complexity on the front end and of course an additional API call is required.
- My recommendation for the response would be to return the full question data up front as the number of topics/misconceptions is probably not going to be massive for a given student. This approach would provide a much smoother user experience as the data is available on the client side when loading the questions for review.


## View
As mentioned above, this probably doesn't need to be a separate endpoint. However, for the sake of completeness, I'll include an example endpoint with a response.

**Assumptions:**
- I am assuming the question images are hosted somewhere accesible via a url.

**Route**: /improve/questions/{id:int}  
**Request:** N/A - id route parameter is sufficient  
**Method:** GET  
**Response:**  
```json
{
  "questionId": 1,
  "questionImageUrl": "string",
  "options": ["A", "B", "C", "D"],
  "correctAnswer": "A",
  "explanation": "string"
}
```







