Retail Insight Data Engineer Recruitment Test
==================================

Thank you for taking the time to do our technical test. It consists of two parts:

* [A technical test](#technical-test)
* [A few feedback questions](#feedback-questions)

Please submit your results by uploading your zipped solution to a supplied dropbox link. In order to obtain the upload URL, please supply your email address to either your agent or the Retail Insight member of staff who assigned you the test.

Please make this a **single** zip file named `{yourname}-{role-applied-for}.zip` containing your solution to the technical test.

## Technical Test

### Technical Introduction
A retail point of sale system at a grocery store handles tens of thousands of item scans per day. This task involves a simplification of the following 4 event types which are part of a retail sales transaction:
- `CheckoutStarted` is raised when all items have been scanned and the customer is asked to pay
- `CheckoutComplete` is raised when the basket is paid for
- `CheckoutAbandoned` is raised when the checkout process is abandoned without payment

`CheckoutStarted` represents the beginning of the workflow. Either one of `CheckoutComplete` or `CheckoutAbandoned` represents the end of the workflow.

The event schema is as follows:

```json
{
    "Type": "CheckoutStarted",
    "Data": {
        "BasketID": "1bb45514-4ce3-448f-8df6-550eeef0c9d0",
        "TimestampUtc": "2020-04-01T13:16:32Z"
    }
}
```

- `Type` is the event type.
- `Data.BasketID` is a randomly generated uuid which uniquely identifies an order. A single order may have more than one event associated with it.
- `Data.TimestampUtc` is the timestamp (iso format) at which the event occurred.

### Implementation
The technical test is to write an app that can generate events and write them to file.

* Each order will produce two events namely (`CheckoutStarted`, `CheckoutComplete`) or (`CheckoutStarted`, `CheckoutAbandoned`).
* Approximately 90% of checkouts will have `CheckoutStarted` and `CheckoutComplete` events and 10% of checkouts will have  `CheckoutStarted` and `CheckoutAbandoned` events.
* The batch of events within a given interval should be stored in a line-delimited JSON file named `orders-<timestamp>.json` eg. orders-2020-04-01-15-05-14-879763.json
* The app must take 4 arguments
    * number-of-checkouts - Number of checkouts to generate (nb. each checkout produces two events).
    * batch-size - Number of events per file.
    * interval - Interval in seconds between each checkout being created.
    * output-directory - Output directory for all created files.
* How to run the app

```bash
generate-events \
    --number-of-checkouts=1000000 \
    --batch-size=5000 \
    --interval=1 \
    --output-directory=<local-dir>
```

* Example shows a snippet of ten events from the content of a generated file. Each event is on its own line separated by a line-break.

```json
{ "Type": "CheckoutStarted", "Data": { "BasketID": "0d25c6ce-f01c-4f39-8d93-168428c92153", "TimestampUtc": "2020-04-14T19:12:32Z" } }
{ "Type": "CheckoutComplete", "Data": { "BasketID": "0d25c6ce-f01c-4f39-8d93-168428c92153", "TimestampUtc": "2020-04-14T19:12:32Z"} }
{ "Type": "CheckoutStarted", "Data": { "BasketID": "4cc0efb8-ce0f-4b36-afe6-f7a6ad3639c7", "TimestampUtc": "2020-04-14T19:12:33Z" } }
{ "Type": "CheckoutComplete", "Data": {"BasketID": "4cc0efb8-ce0f-4b36-afe6-f7a6ad3639c7", "TimestampUtc": "2020-04-14T19:12:33Z"} }
{ "Type": "CheckoutStarted", "Data": { "BasketID": "b7bfb039-8779-48f5-86fa-808ae277a503", "TimestampUtc": "2020-04-14T19:12:34Z" } }
{ "Type": "CheckoutComplete", "Data": { "BasketID": "b7bfb039-8779-48f5-86fa-808ae277a503", "TimestampUtc": "2020-04-14T19:12:34Z"} }
{ "Type": "CheckoutStarted", "Data": { "BasketID": "84793616-4715-4e58-910b-4298cf24b632", "TimestampUtc": "2020-04-14T19:12:35Z" } }
{ "Type": "CheckoutComplete", "Data": {"BasketID": "84793616-4715-4e58-910b-4298cf24b632", "TimestampUtc": "2020-04-14T19:12:35Z"} }
{ "Type": "CheckoutStarted", "Data": { "BasketID": "c93fc4f3-5b1f-446a-9438-55f1ce0f0a3e", "TimestampUtc": "2020-04-14T19:12:36Z" } }
{ "Type": "CheckoutComplete", "Data": { "BasketID": "c93fc4f3-5b1f-446a-9438-55f1ce0f0a3e", "TimestampUtc": "2020-04-14T19:12:36Z"} }
{ "Type": "CheckoutStarted", "Data": { "BasketID": "99cf9047-80db-4361-869f-cbbd39fef463", "TimestampUtc": "2020-04-14T19:12:37Z" } }
{ "Type": "CheckoutAbandoned", "Data": {"BasketID": "99cf9047-80db-4361-869f-cbbd39fef463", "TimestampUtc": "2020-04-14T19:12:37Z"} }
```

* Every `CheckoutStarted` event must have a `CheckoutComplete` or `OrderCancelled` event with the same `BasketID`.

### Platform Choice

You can create the solution in any language or framework of your choice. Please don't use a language that you're unfamiliar with. We want to see you at your best!

# Feedback Questions

Feel free to spend as much or as little time on the exercise as you like. We would really like you to show us how you code. We are looking for something that works, includes best practice, and is readable and easy to follow and review.

Please create a README.md file in the root directory.

in the README:

1. Provide clear instructions on your test setup and how to execute your tests.
1. List any additional comments or observations you might want to share about your submission.
1. How long did you spend on the coding test?
1. How would you improve your solution if you had more time? If you didn't spend much time on the coding test then use this as an opportunity to explain what you would add.
1. Why did you choose the language you used for the coding test?

#### Thanks for your time, we look forward to hearing from you!
- The [Retail Insight Engineering team](https://ri-team.com)