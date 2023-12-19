
# Database Structure

Table 1: Users

| Field               | Type         | Constraints                            |
|---------------------|--------------|-----------------------------------------|
| uid                 | varchar(255) | PRIMARY KEY                            |
| username            | varchar(50)  |                                         |
| password            | varchar(255) |                                         |
| department          | varchar(50)  |                                         |
| email               | varchar(255) | UNIQUE                                  |
| verification_status | int          |                                         |
| verification_code   | varchar(255) |                                         |
| code_expiry_time    | datetime     |                                         |

Table 2: Conversations

| Field             | Type         | Constraints                            |
|-------------------|--------------|-----------------------------------------|
| uid               | varchar(255) | FOREIGN KEY (uid) REFERENCES Users(uid) |
| conversation_id   | varchar(255) | PRIMARY KEY                            |
| created_time      | timestamp    | DEFAULT CURRENT_TIMESTAMP               |
| conversation_name | text         |                                         |

Table 3: Messages

| Field                 | Type         | Constraints                            |
|-----------------------|--------------|-----------------------------------------|
| conversation_id       | varchar(255) | PRIMARY KEY                            |
| message_id            | int          | PRIMARY KEY                            |
| message_content       | text         |                                         |
| files_ref             | text         |                                         |
| corrections           | json         |                                         |
| message_functionality | varchar(255) | DEFAULT 'normal'                       |

Table 4: Documents

| Field            | Type         | Constraints         |
|------------------|--------------|----------------------|
| uid              | varchar(255) | PRIMARY KEY          |
| file_name        | varchar(255) | PRIMARY KEY          |
| file_description | text         |                      |
| file_summary     | text         |                      |

Table 5: Common Documents

| Field            | Type         | Constraints                                                |
|------------------|--------------|-------------------------------------------------------------|
| file_name        | varchar(255) | PRIMARY KEY                                                |
| file_description | text         |                                                             |
| last_edited      | timestamp    | DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP       |
| file_summary     | text         |                                                             |

Table 6: Feedback

| Field            | Type         | Constraints                                                            |
|------------------|--------------|-------------------------------------------------------------------------|
| conversation_id  | varchar(255) | PRIMARY KEY                                                            |
| message_id       | int          | PRIMARY KEY                                                            |
| feedback_type    | int          | DEFAULT 0                                                              |
| feedback_content | text         |                                                                         |
| seen_status      | bit(1)       |                                                                         |
| created_at       | timestamp    | DEFAULT CURRENT_TIMESTAMP                                              |
| updated_at       | timestamp    | DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP                 |

Table 7: Average response time

| Field         | Type         | Constraints                            |
|---------------|--------------|-----------------------------------------|
| month         | int          | PRIMARY KEY                            |
| num_messages  | int          |                                         |
| response_time | float(10,3)  |                                         |