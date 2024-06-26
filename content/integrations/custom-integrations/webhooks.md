---
title: Webhooks
order: 3
---

# Webhooks
:::important Premium Feature
Webhooks are only available on our custom plans.
:::

Webhooks make it easy to tie into events that occur in the Qualified system. You can configure them on the [integrations page](https://www.qualified.io/hire/account/integrations#webhooks).


## Setup

Want to be notified when a candidate starts and finishes their assessment? Interested in monitoring your own team’s development of challenges and assessments?

Simply provide a URL to your API and check the boxes for the events you want to be broadcasted to this URL. Then you can integrate into your own system actions to be taken upon completion of these events.

Multiple webhook subscriptions are supported, to allow for multiple systems to be notified.

## Retries

There are certain conditions in which a failed webhook delivery attempt will be retried. Webhook events will only be retried once in these cases. 

The following cases result in a single retry attempt being made:
- Connection failure to the webhook URL
- Connection timeout (after 5 seconds)
- Server returns one of the following status codes: `502`, `503`, `504`, `520`, `521`, `523`, `525`, `526`, `527`, `530`

The retry attempt will wait at least 60 seconds before retrying. 

The reason why Qualified only retries once under certain conditions is to minimize the potential for duplicate data to be created within the receiving app. Sometimes failures happen after a certain amount of processing has already happened and can cause
the receiving application to respond to retries as if it is a new event. 

## Event Management
For each configured webhook subscription, you can view the last 100 webhook events that were sent. From this interface you can manually trigger a retry for any of the failed events, as many times as you need to.

## Trigger Events

Each webhook subscription can be configured to trigger for specific events. We recommend only configuring the triggering events that you need, in order to minimize unnecessary server load.


### Assessment Events

These methods are specific to updates concerning assessments so they emit a [Assessment](#assessment) in the `data` property.

- **Assessment Created** (assessment_created) - This event will fire when an assessment has been created.
- **Assessment Updated** (assessment_updated) - This event will fire when an assessment has been updated.
- **Assessment Published** (assessment_published) - This event will fire when an assessment has been published. At this point it will become available for candidate usage.
- **Assessment Destroyed** (assessment_destroyed) - This event will fire when an assessment has been deleted. This is a soft delete, so the assessment will in fact continue to exist should you need to reference it later.

### Assessment Result Events

These methods pertain specifically to a candidate's assessment outcome so they will emit an [Assessment Result](#assessment-result) in the `data` property.

- **Assessment Result Started** (assessment_result_started) - This event will fire when a candidate selects their language and begins their challenge on an assessment result.
- **Assessment Result Submitted** (assessment_result_submitted) - This event will fire when a candidate has completed their assessment and clicks submit. Once this has occurred, the candidate can no longer update the assessment result.
- **Assessment Result State Changed** (assessment_result_state_changed) - This event will fire when any of the following state changes occur on the assessment result: `invited`, `opened`, `started`, `submitting`, `submitted`, `terminated`. When using this event, you should not use `assessment_result_started` or `assessment_result_submitted` as it would be redundant.  

:::important Deprecation Notice
`team_candidate_started_assessment` and `team_candidate_submitted_assessment` events are deprecated. If you are currently using these events, please
update your integration to use `assessment_result_started` and `assessment_result_submitted`.
:::

### Assessment Result Solution Events

These methods pertain specifically to a candidate's solution within an assessment so they will emit a [Solution](#solution) in the `data` property.

- **Solution Created** (solution_created) - This event will fire when a candidate starts to work on their solution for a challenge.
- **Solution Updated** (solution_updated) - This event will only fire when a candidate tests or submits their code solution. For Q&A solutions, this event will fire on all updates.

### Assessment Result Review Events

These methods reference the assessment result that is a team member's evaluation of a candidate's assessment result. It will an emit an [Assessment Result Review](#assessment-result-review) in the `data` property.

- **Assessment Result Review Created** (assessment_result_review_created) - This event will fire when an assessment result review has been created by a team member.
- **Assessment Result Review Updated** (assessment_result_review_updated) - This event will fire when an assessment result review has been updated by a team member.
- **Assessment Result Review Submitted** (assessment_result_review_submitted) - This event will fire when an assessment result review has been submitted by a team member.
- **Assessment Result Review Destroyed** (assessment_result_review_destroyed) - This event will fire when an assessment result review has been deleted. This is a permanent delete, so further references to the challenge may not be valid.

### Challenge Events

These methods are specific to updates concerning challenges so they emit a [Challenge](#challenge) in the `data` property.

- **Challenge Created** (challenge_created) - This event will fire when a team member has created a new challenge. It will emit an [Challenge](#challenge) as it's `data`.
- **Challenge Updated** (challenge_updated) - This event will fire when a team member has updated a challenge.
- **Challenge Published** (challenge_published) - This event will fire when a challenge is published. At this point the challenge will become available for candidate usage.
- **Challenge Destroyed** (challenge_destroyed) - This event will fire when a challenge is deleted. This is a permanent delete, so further references to the challenge may not be valid.

### Team Candidate Events

These methods are specific to updates concerning team candidates so they emit a [TeamCandidate](#team-candidate) in the `data` property.

- **Team Candidate State Changed** (team_candidate_state_changed) - This event will fire whenever a team candidate's state changes. This includes when a team candidate is first created, due to an invite or starting an assessment via a public link.
- **Team Candidate Label Added** (team_candidate_label_added) - This event will fire whenever a team candidate has a label added. If multiple labels are added at the same time, a unique event for each one will be fired.
- **Team Candidate Label Removed** (team_candidate_label_removed) - This event will fire whenever a team candidate has a label removed. If multiple labels are removed at the same time, a unique event for each one will be fired.

### Pathway Result Events

These methods are specific to updates concerning team candidates so they emit a [TeamCandidate](#team-candidate) in the `data` property.

- **Pathway Result Created** (pathway_result_created) - This event will fire whenever a team candidate is invited to a Pathway and a Pathway Result is created for them.
- **Pathway Result State Changed** (pathway_result_state_changed) - This event will fire whenever a Pathway Result's state changes.

### Pathway Step Result Events

These methods are specific to updates concerning team candidates so they emit a [TeamCandidate](#team-candidate) in the `data` property.

- **Pathway Step Result Created** (pathway_step_result_created) - This event will fire whenever a Pathway Step Result is created on creation of their Pathway Result.
- **Pathway Step Result State Changed** (pathway_step_result_state_changed) - This event will fire whenever a Pathway Step Result's state changes.

### Pathway Step Result Events

These methods are specific to updates concerning team candidates so they emit a [TeamCandidate](#team-candidate) in the `data` property.

- **External Evidence Invite Created** (external_evidence_invite_created) - This event will fire whenever a Pathway Step Result is activated with an External Evidence option, creating and returning an External Evidence Result related to it.


## Simulating and Firing events for development

See the API docs at https://qualified.io/api/docs#webhook-examples for a set of APIs for simulating and triggering webhook events.

## Data Models

The data sent emitted by the configured webhooks can be broken down according to their data models.

### Assessment

The assessment represents the set of challenges that a candidate can be presented with.

**Properties**

- title
- summary
- purpose
- challenges: Set of [challenges](#challenge)

### Assessment Result

The assessment result represents a candidate's outcome after taking their challenges.

**Properties**

- id
- score
- accumulatedSeconds
- workflowName
- abandoned
- state
- candidateRemarks
- passingScore
- invitedAt
- startedAt
- submittedAt
- candidate
- teamCandidate
- assessment
- solutions: Set of [solutions](#solution)

### Assessment Review

The assessment review represents a team member's evaluation of a candidate's assessment result.

**Properties**

- feedback
- rating
- state
- submittedAt
- reviewedById: The team member who reviewed this candidate
- teamCandidateId: The candidate who has received this review
- assessmentResultId: The associated assessment result
- solutionReviews: A set of [solution reviews](#solution-reviews)

### Challenge

Represents a challenge within your library.

**Properties**

- id
- _type
- title
- estimatedType
- topics
- summary
- purpose

### Pathway Result

Represents a candidate's attempt at a Pathway.

**Properties**

- id
- labels
- state
- updatedAt
- createdAt
- passedAt
- progressStartedAt
- closedAt
- invitedAt
- failedAt
- terminatedAt
- pathway: The pathway this pathway result pertains to
- teamCandidate: The team candidate this pathway result pertains to
- pathwayStepResults: The set of [pathway step results](#pathway-step-results) pertaining to these pathway result

### Pathway Step Results

Represents the step results pertaining to a pathway result.

**Properties**

- id
- pathwayResultId
- state
- assessmentId
- externalEvidenceId
- createdAt
- passedAt
- failedAt
- skippedAt
- skipReasonNote
- erroredAt
- passing
- flagged

### External Evidence Result

Represents the result to an external evidence on a pathway step's option.

**Properties**

- id
- passed
- url: URL for the invitation
- invitationId
- content
- metadata

### Solution

Represents a candidate's attempt to solve a particular challenge.

**Properties**

- id
- _type
- score
- passedTestCases
- totalTestCases
- weightedTestCases
- attempts
- language
- firstTestCaseSolvedAt
- completedAt
- wallTime
- challenge: The [challenge](#challenge) this solution pertains to

### Solution Reviews

Represents a team member's review of a particular candidate's challenge solution.

**Properties**

- rating
- feedback
- challengeId: The id of the [challenge](#challenge) for this review
- solutionId: The id of the [solution](#solution) for this review

### Team Candidate

Represents the developer taking the assessment. For education customers, this entity is called a "Student" within the UI.

**Properties**

- name
- firstName
- lastName
- anonymouseName
- email
- state
- prevState
- labels: Array of strings
- testTeamMember: true if the team candidate is actually a team member taking the assessment)
- highestScore
- recentAssessmentResult: assessment result with the most recently updated state
- assessmentResultsCount
- addedLabel: Only present for `team_candidate_label_added` event
- removedLabel: Only present for `team_candidate_label_removed` event
