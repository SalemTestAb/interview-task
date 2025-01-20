# Programming Assignment, Omegapoint

This assignment aims to assess your software development
knowledge and ability to translate requirements into secure and robust code. There is
also an extension of the assignment for more experienced and senior hires.

## Technical Requirements

- The solution must be developed in one of the following programming languages: Java, C#, JavaScript, TypeScript, Python, C++, Rust, or Ruby.
- Utilize your chosen language's constructs for generalization and composition where appropriate.
- You may only use standard libraries.
- The assignment solution should include a markdown file called `solution` containing documentation such as how to run the program and any assumptions affecting the solution.
- The assignment solution should include a build script that compiles the main program, which can be used to execute the code.
- Submit the assignment by pushing your code to a branch in the repository following the naming format `<your-first-name>-<your-last-name>`

## Assignment Overview: Secure Validation of Swedish Identification Numbers

Implement a solution that validates various Swedish identification numbers, including personal numbers, coordination numbers, and organization numbers. Your code should ensure correct formatting and integrity checks while also handling invalid or malicious input without compromising system integrity or security.

## Assignment Details

Your task is to **validate Swedish personal numbers** and related identifiers. Your program will receive input as strings, and you must check whether the string represents a valid identifier number. All validation calculations should adhere to security best practices, ensuring that edge cases and invalid inputs are handled securely.

### Swedish Personal Number Identifier

A valid Swedish personal number consists of:

- Six digits representing the birthdate (YYMMDD).
- Four digits representing a serial birth number.

Special cases include:

- Numbers for individuals born more than 100 years ago may use either a "+" as a separator or include the century prefix in the birthdate (18, 19, 20).
- A 10-digit personal number without a separator assumes the person is younger than 100 years.

### Additional Identifiers

- **Coordination Numbers**  
  Coordination numbers follow the same rules as personal numbers but with a slight variation: 60 is added to the day component (valid dates can then be between 61 and 91). This allows the system to accommodate individuals who need identification but do not have a personal number. Your solution should securely handle this nuance and prevent invalid coordination numbers from being accepted.

- **Organization Numbers**  
  Organization numbers are used to identify legal entities, such as companies or associations. The first digit indicates the type of organization, while the middle digit pair must be at least 20. Additionally, the century can be specified as "16", in a 12-digit form. The solution for this should be able to handle organization numbers and ensure that invalid inputs cannot bypass validation.

### Validity Checks

A requirement in the code is the implementation of **ValidityCheck** functions to ensure the integrity of these identifiers. These checks should be structured to handle validation securely and extensible. Any failure should be logged with clear details on why the check failed to support auditing and traceability.

### Luhn's Algorithm

For all identifiers, validation must include **Luhn’s algorithm**, ensuring compliance with integrity standards. Implement the algorithm securely, avoiding potential off-by-one errors or improper handling of edge cases.

#### Luhn's Algorithm formula/steps

**Step 1:** Starting from the right, double each second-to-last digit. If the result of any doubling operation is 10 or greater, then add the digits of the result to obtain a single-digit number.
Example: 6 × 2 = 12; 1 + 2 = 3

**Step 2:** Calculate the sum of all digits you didn’t double and the new values you got from doubling.

**Step 3:** Determine if the total sum is a multiple of 10. The number is considered valid according to the Luhn's algorithm if the total ends in 0.

**Example** for the number 19710116-9295:

1. Double every other digit starting from the right:

|        |     |     |     |     |     |     |     |     |     |     |
| ------ | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|        | 7   | 1   | 0   | 1   | 1   | 6   | 9   | 2   | 9   | 5   |
| \*     | 2   | 1   | 2   | 1   | 2   | 1   | 2   | 1   | 2   | 1   |
| Result | 14  | 1   | 0   | 1   | 2   | 6   | 18  | 2   | 18  | 5   |

2. Sum all digits: `1 + 4 + 1 + 0 + 1 + 2 + 6 + 1 + 8 + 2 + 1 + 8 + 5 = 40`

3. Validate that the result is a multiple of 10: `40 % 10 = 0`

### Example Data

- **Valid Personal Numbers**
  These numbers are taken from Skatteverkets test data and do not belong to existing people.

  - 201701102384
  - 141206-2380
  - 20080903-2386
  - 7101169295
  - 198107249289
  - 19021214+9819
  - 190910199827
  - 191006089807
  - 192109099180
  - 4607137454
  - 194510168885
  - 900118-9811
  - 189102279800
  - 189912299816

- **Invalid Personal Numbers**

  - 201701272394
  - 190302299813

- **Valid Coordination Numbers**

  - 191500722390
  - 191611882398
  - 191711602399
  - 191800852384
  - 191911612388
  - 192001602396
  - 192112792383

- **Valid Organization Numbers**
  - 556614-3185
  - 16556601-6399
  - 262000-1111
  - 857202-7566

## Extension of Assignment for Experienced or Senior Developers

The extension assignment aims to assess your knowledge of software development along with common tools and practices in the field of software development. Please try and follow common coding principles and patterns and leave comments both for documentation but also explaining design choices and trade-offs.

### Extension Assignment: Persisting Data and Validating Using External APIs

Implement the functionality to:

- Persist validated identifiers to a MySQL database.
- To extract persisted personal numbers based on birth year.

An additional part of validating the personal and coordination numbers should be to ensure that they are not one of the test numbers provided by Skatteverket.

- Endpoint documentation for Personal Numbers:  
  [Person Numbers API](https://swagger.entryscape.com/?url=https%3A%2F%2Fskatteverket.entryscape.net%2Frowstore%2Fdataset%2Fb4de7df7-63c0-4e7e-bb59-1f156a591763%2Fswagger#/)

- Endpoint documentation for Coordination Numbers:  
  [Coordination Numbers API](https://swagger.entryscape.com/?url=https%3A%2F%2Fskatteverket.entryscape.net%2Frowstore%2Fdataset%2F9f29fe09-4dbc-4d2f-848f-7cffdd075383%2Fswagger)

This extension assignment has fewer defined requirements and may be interpreted more creatively when implemented, as long as any design and implementation choices can be justified. You may use more than just the standard libraries.

### MySQL Integration and Setup Using [Docker](https://www.docker.com/)

To assist in persisting data, the repository contains a [Dockerfile](./Dockerfile) to be used for the MySQL database and a SQL [initialization file](./init.sql).

- Build the image:  
  `docker build -t <chosen image tag/name> .`

- Validate build:  
  `docker images`

- Start the image/Run the Database:  
  `docker run -d -p 3306:3306 <chosen image tag/name>`

- Investigate/Run commands on the image:  
  `docker exec -it <running image id> bash`

**Database credentials:**

- **Username:** application_user
- **Password:** 1234
