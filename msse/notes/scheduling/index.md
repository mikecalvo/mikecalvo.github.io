footer: © Citronella Software Ltd 2015
slidenumbers: true

# Scheduling
## Mike Calvo
### mike@citronellasoftware.com

---
# Running Scheduled Code
- Needs for scheduling functionality
  - Updating local copies of external data
  - Cleaning up old data
  - Large volume data transactions
  - Generating reports
  - Sending marketing emails

---
# Quartz
- Established Java scheduling library
- Job: functionality to run on a schedule
- Trigger: when to run a job
- Styles of job triggers
  - Simple: start delay, repeat interval, repeat count
  - Cron: Powerful UNIX style scheduler
  - Custom

---
# Grails Quartz Plugin
`compile ":quartz:1.0.2"`

- Adds concept of Job to Grails
`grails create-job MyJob`
- Jobs live in grails-app/jobs

---
# Example Job

```
class MarketingEmailJob {
  static triggers = {
    cron name: 'daily', cronExpression: '0 0 2 * * ?'
  }

  def group = 'Marketing'
  def description = 'Send annoying emails to customers'
  def execute() {
    // Do my job
  }
}
```

---
# Trigger Block
- Static job sceduling
- Specify:
  - type (simple, cron, custom)
  - name
  - Type-dependent paramers
- Multiple can be defined

---
# Trigger Examples

```
class MyJob {
  static triggers = {
    simple name:'simpl', startDelay:10000, repeatInterval: 30000, repeatCount: 10
    cron name:'cronTrigger', startDelay:10000, cronExpression: '0/6 * 15 * * ?'
    custom name:'customTrigger', triggerClass:MyTriggerClass, param:value
  }
  def execute() {
    println "Job run!"
  }
}
```

---
# Dynamic Job Scheduling
- Job classes have a schedule method added to them:
  - by cron: MyJob.scheudle('0 0 2 * * ?)
  - by simple: MyJob.schedule(1000, 2000, [param: 'value'])
  - by trigger: MyJob.schedule(TriggerClass)
  - by Date: MyJob.schedule(dateInstance)
- Trigger now: MyJob.triggerNow([maxValues: 300])

---
# Plugin Configuration
- grails-app/conf/QuartzConfig.groovy

```
quartz {
  autoStartup = true
  jdbcStore = false
}
```

---
# Job Config Properties
- sessionRequired: do (or not) require a GORM session
- concurrent: do (or not) start multiple concurrent jobs
- description: human-readable description
- requestsRecovery: re-execute in event of failure
