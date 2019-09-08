---
title: "Book Notes: Data Mining For Dummies"
date: 2017-11-23T18:58:05+03:00
description: ""
tags:
categories: ["datascience", "book_notes"]
type: post
draft: false

---

My reading notes from the book

<!--more-->

<!-- toc -->

## Introduction

    written for
      business users
      heard a little about dm


## ch01: Catching the Data-Mining Train

    Getting Real about DM
      Not your professor's statistics
      value of dm
        ex of information
          retailer loyalty program
            which customers are likely to spend 
              a lot
              a little
            based on information gathered from first visit
          manufacturer: accidental release of toxic materials
            prevent dangerous accidents
          insurance: 
            an office processes certain claim types more quickly
            right place for best practices
          advertising
            which ad works better?
              one that has female face, or male face
              many images vs. a few
              same copy but different layouts
    Doing what DM do
      focusing on business
      understanding their time
        most time: data preparation
      process
        crisp-dm
          business understanding
          data understanding
          data preparation
          modeling
          evaluation
          deployment
      making models
        report 
          can show
            sales down by region, channel
            declines are widespread?
          cannot
            why sales declined
            what actions to take
        model
          factors that impact sales
          actions that increase sales
      understanding mathematical models
      putting information into action
    Discovering Tools and Methods
      visual programming
      working quick
      testing


## ch02: A Day in Your Life As data Miner

    Starting your day off right
    Understanding your business
    understanding your data
      project
        predict land ownership
      explore data and document it
        ranges of variables
        missing values
        not all variables are documented
        ex
          variable name | type | missing | range/summary | min | max | median
          taxkey | integer | 30 | histogram |
        describe variable summaries
          ex
            variable | description
            bi_viol 
              description: unknown
              type: string
              range: x to xxx
              missing: 0
              assessment: not good for modeling. all cases have same value. reason unknown
              next: won't use in this project
            taxkey
              assessment: small number of cases missing. some have less than 10 digits. probably due to leading zeros
              next: clean this variable
            ca_class
              assess: good
    Deriving new variables
      flow
        read csv -> select atributes -> filter examples -> cut -> generate attributes -> select attributes -> sample -> write csv
      filtering out cases
        condition: no missing
      cutting variables down to five characters
        attribute filter type: subset
        attributes: ...
        first character: 1
        last character index: 5
      functions for generating new variables
        attribute name: ntlocal
        function expressions:
          if ( geo_zip_code == owner_zip, 0, 1 )
      discard variables no longer needed
      balancing the data
        sample size per class
          class | size
          yes | 4000
          no | 4000
    Modeling Your Data


## ch03 Teaming Up To Reach Your Goals

    Nothing could be finer than to be a data miner
      You can be a data miner
        ex: 2
          public safety: NY Fire Department
            identify factors that put buildings at risk for fire
            output: risk score for 300K buildings
            use: inspect building with risk
          retail
            amazon.com
            individualize product recommendations
            test functional and cosmetic aspects of website
          medical and survey research
            smoking
            identify messages that effectively discourage youth from smoking
      Using the knowledge you have
    Data Miners Play Nicely with Others
      Cooperation is a necessity
      Oh, the people you'll meet
    Working with Executives
      Greetings and elicitations
      Lining up your priorities
      Talking data mining with executives
          


## ch04 Learning the Laws of Data Mining

    1. law: business goals
      business objectives are the origin of every data mining solution
    2. law: business knowledge
      business knowledge is central to every step
    3. law: data preparation
      data preparation is more than half of every dm process
    4. law: right model
      right model for a given application can only be discovered by experiment
      NFL-DM: no free lunch for data miner	what is a model
        mathematical relation
          represents a pattern
            observed in data
      Tom Khabaza
    5. law: pattern
      there are always patterns
      successful exploration begins with a goal
        cook, peary -> north pole
    6. law: amplification
      dm amplifies perception
        enables you to understand your business better than without it
        like a magnifier or microscope
    7. law: prediction
      prediction increases information locally by generalization
      ex
        customer enters store
        how much will customer spend?
        you don't know him
          best estimate: average amount
        he heads for electronics
          estimate: higher
    8. law: value
      value of dm results is not determined by accuracy or stability of predictive models
      dm
        don't fuss over theory
        uses testing rather than statistical theory to justify
      stats
        fuss over theory
        accuracy and stability important
    9. law: change
      all patterns are subject to change


## ch05: Embracing the Data-Mining Process

    Whose standard is it, anyway?
      crisp-dm standard
        iterative
        with smaller cycles
      documenting your work
    Business Understanding 
      identifying your business goals
        problem that management wants to address
        business goals
        constraints (limitations)
        impact (how problem and possible solutions fit in with the business)
      deliverables
        background
          2-3 paragraphs
          ex
            our client, regional planning commission, wants to influence property use
              only advisory
              no independent power
            best opportunity: when property changes hands
              best time: before property is about to change ownership
            factors believed to indicate change of ownership:
              nonlocal ownership, code violations, foreclosure...
        business goals
          broader goal than dm project
          ex
            increase sales from ad campaign by 10%
        business success criteria
          how the results will be measured
          get quantitative criteria
      assessing your situation
        go fact-finding
        explain issues 
        deliverables
          inventory of resources
            people, data, software
          requirements, assumptions, and constraints
            schedule for completion
            legal and security obligations
            requirements for acceptable finished work
          risks and contingencies
            risk for delay
              contingency plan for them
          terminology
            business terms, dm terms
          costs and benefits
      defining your data-mining goals	
        deliverables
          data-mining goals
            models, reports, presentations, datasets
          data-mining success criteria
            define in quantitative terms
              model accuracy or predictive improvement
            if qualitative:
              who will make the assessment
      producing your project plan
        deliverables
          project plan
            step by step action plan
            schedule
            for each step
              required resources, 
              inputs (data), 
              outputs (model, data, report)
              dependencies
          initial assessment of tools and techniques
    Data Understanding
      gathering data
        deliverables
          initial data collection report
            verify that
              you acquired data or
              gained access to data
                tested data access process
                verified data exists
            work needed
              outline data requirements
                types of data
                with details
                  time range, data formats
              verify data availability
                if some data unavailable
                  how to substitute it
                  narrowing the scope
                  gathering new data
              define selection criteria
                data sources you will use
                which tables, fields etc.
            you must actually obtain data
              import it to dm tool
              make trials
              possible issues
                limits on cases/memory
                inability to read data formats
                imperfections of data
      describing data
        deliverables
          data description report
            sourtce and formats of data
            number of cases
            number and descriptions of fields
            suitabilityof data for dm goals
      exploring data
        examine data more closely
        data exploration
          range of values, distributions
        deliverables
          data exploration report
            distributions, summaries, data quality problems
      verifying data quality
        deliverables
          data quality report
            data you have
            minor, major quality issues
            possible remedies
              ex: alternative data resource
    Data Preparation
      5 tasks
        selecting data
          deliverable
            rationale inclusion and exclusion
              what data will be used or not
                reasons based on
                  relevance
                  data quality
                  technical issues
                  suitability of data formats
        cleaning data
          deliverables
            data cleaning report
              in excruciating detail
              every decision and action to clean data
        constructing data
          deliverables
            derived attributes
              new fields constructed
                how, why
            generated records
              new cases (rows) 
                how, why
        integrating data
          deliverables 
            merged data
              how performed
        formatting data
          deliverables
            reformatted data
              how performed
    Modeling
      most liked
      4 tasks
        selecting modeling techniques
          deliverables
            modeling technique
              specify the technique
            modeling assumptions
        designing tests
          how well model works
          avoid overfitting
          holdout data
            not used during model-training process
          deliverables
            test design
              not elaborate
        building models
          deliverables
            parameter settings
            model descriptions
              describe model
                type of model (linear, neural)
                variables used
              how it is interpreted
              difficulties encountered
            models
        assessing models
          deliverables
            model assessment
            revised parameter settings
    Evaluation
      3 tasks
        evaluating results
          deliverables
            assessment of results
              did you reach the business goals?
            approved models
        reviewing the process
          spot issues overlooked
          how to improve process?
          deliverables
            review of process report
              outline review process
              findings and concerns for immediate attention
                steps overlooked or should be revisited
        determining the next steps
          recommendations for next move
          deliverables
            list of possible actions
            decision
    Deployment
      4 tasks
        planning deployment
          deliverables
            deployment plan
              steps required
                instructions
        planning monitoring and maintenance
          deliverables
            monitoring and maintenance plan
        reporting final results
          deliverables
            final report
              summary: entire project 
                assemble all reports
                add overview
            final presentation
        reviewing project
          deliverables
            experience documentation report


## ch06: Planning for Data Mining Success

    Setting the Course with Formal Business Cases
      business case
        to justify costs, prepare business case
        what is
          outlines business problem
          proposed plan to address it
          benefits and costs
        helps you too
          clarifies thinking
      Satisfying the boss
      Minimizing your own risk
    Building Business Cases
      Elements of the business case
        elements
          background
            what organizations is involved?
            what is its business?
          problem statement
            what is wrong?
            when did it start?
            whom does it affect?
            is the cause known?
            is this a common or unusual type of problem?
          action alternatives
            what solutions are suggested?
            benefits and costs?
          preferred action
            best?
            make your case
          connection of preferred action with strategic goals
        benefits
          expected benefits
          mechanism
            how will action cause benefits?
          metrics?
            how to measure benefit
        costs
          costs
          cost of taking no action
      Putting it in writing
        executive summary
          for all cases of 3+ pages
      The basics on benefits
    Avoiding the Failure Option


## ch07: Gearing Up with the Right Software

    Putting DM Tools in Perspective
    Evaluating Software


## ch08: Digging into Your Data

    Focusing on a Problem
    Managing Scope
    Using Your Organization's Own Data
      data collected from common business activities
        research
          competitor product information
          experimental and test data
        manufacturing
          process data
          procurement records
          production records
          inspection and test records
        marketing
          competitor marketing information and sales data
          campaign data
          marketing cost data
        sales
          sales activity
          sales data
          customer information
        fulfillment
          packaging records
          shipping records
          shipping complaints
        customer service
          customer interaction records
          product and service complaints
          service issues
        technical support
          support requests
          product problem reports
          design and other product suggestions
        training
          staff training records
          customer training records
          certification and other credentialing records
        accounting 
          bills
          payments 
          audit records
          taxes collected and paid


## ch09: Making New Data

    Loyalty Programs
      loyalty program
        agreement between business and customers
        customers allow business to track purchases
        business offers rewards
      your data bonanza
        data elements in retail sector
          customer location
          products purchased
          combinations of products purchased together
          prices paid
          list of everyday prices 
          coupon or other discount offer used
          time
          detailed product descriptions
          pages/products viewed
          time on site
          timing of site visits
          product reviews and information sharing
          referrals
          offers or ads customer viewed
          social network details - people customer knows
        what is important to a particular decision maker?
          how to figure it out?
          learn executive's responsibilities
          find out metrics most important to his survival
          mine data for clues what actions could increase sales
            ex: loyalty programs
              characteristics of customers 
                who buy large
                increase the amount
              growing customer segments
              combinations of products bought together
              promotions that work better than others
              marketing channels more cost-effective
              shopper behavior patterns (instore and online) that affect sales
              unexpected factors that influence sales
        warehouse clubs
          people pay to be a member
    Testing
      experimenting in direct marketing
        most common application for experiments
        names:
          A/B tests
          split tests
        ex
          retailer sends emails to customers who haven't purchased in 24 hours
          changes to email message improve response?
      direct marketing
        everything is direct marketing if it is an action per person
    Microtargeting to win elections
      microtargeting
        organized survey research, testing
          to deliver personalized campaign messaging
      Treating voters as individuals
      Enhancing voter data
        new information
          demographics
          occupation
          memberships
          home, auto ownership
          permits
          magazine subscriptions
      Developing your own test data
    Surveying the Public Landscape
    Getting into the Field
    One Challenge, Many Approaches


## ch10: Ferreting Out Public Data Sources

    Exploring Public Data Sources
      www.fedstats.gov
        find agency by name or subject
      www.data.gov
        not a data source
        information about what data is available
      most popular data sources
        climate data online
        consumer complaint database
        noaa national weather service
        federal student loan program data
        state education data profiles
        social media monitoring metrics
        food access
        trade in goods and services
        campus crime data
        dropout and completion of schools
      Bureau of Economic Analysis
        www.bea.gov
        part of Department of Commerce
          12 agencies
        data sources
          balance of payments
          foreign direct investment
          gdp
          industry data
      Bureau of Justice Statistics
        www.bjs.gov
          data sources
            crime and victims
            drugs
            criminal offenders
            courts
            corrections
            employment 
      Bureau of Labor Statistics
        www.bls.gov
        data sources
          compensation
          consumer expenditures
          workers
          cost trends
          foreign labor
          unemployment
          productivity
          wages
      Bureau of Transportation Statistics
        www.rita.dot.gov/bts
        data sources
          airlines
          commodity flow
          freight
          travel
      Census Bureau
        www.census.gov
        part of Commerce
        lives of Americans
        data sources
          business ownershipg
          international trade
      Economic Research Service
        www.ers.usda.gov
        data
          biotech
          agribusiness
          crops
          trade
      Energy Information Administration
        www.eia.gov
        D of Energy
      Environmental Protection Agency
        www.epa.gov
      Ofice of Research, Analysis and Statistics
        www.irs.gov/uac
        part of Internal Revenue Service IRS
          tax collection agency
        data
          tax
      National Agricultural Statistics Service
        NASS
        www.nass.usda.gov
      National Center for Education Statistics
        nces.ed.gov
      National Center for Health Statistics
      National Science Foundation
      Office of Management and Budget
      Office of Retirement and Disability Policy
    Governments of world
      Offstats
        stats of world govs
      OECD
      US open gov portal
      UN
      EU
    US State and Local govs
      freedom of information act
      pew charitable trusts
      us counties
        www.data.gov/counties
      us cities
        www.data.gov/cities


## ch11: Buying Data

    Peeking at Consumer Data
      Axciom
        aboutthedata.com
        individuals
        households
    Beyond Consumer Data
    Desperately Seeking Sources
      professional associations for making contacts
        marketing association
        list of data vendors: appendix c


## ch12: Getting Familiar with Your Data

    Importing Data
      in Knime
      in Weka
      procedure
        CSV reader
    stats
      nodes > statistics


## ch13: Dealing in Graphic Detail

    Eyaballing variables with histograms
      RapidMiner
        data summary tool
        charts > bar chart
      relating variables with scatterplots
        mpg vs. horsepower
      interacting with scatterplots
        select area
        sampling randomly
        dataset created by selection in graph
        selection shapes:
          rectangle, free, polyline


## ch14: Showing Your Data Who's Boss

    Rearranging Data
      variable order
    sorting



## ch15: Your Exciting Career in Modeling



## ch16: Data Mining using Classic Statistical Methods



## ch17: Mining Data for Clues

    market basket analysis
      understanding the metrics
        diagnostics
        ranking
        lift


## ch18: Expanding Your Horizons

    Using meta models
      ensemble model
      using 2+ modeling techniques together
    Widening your range
      tackling text
        text mining
        uses
          sentiment analysis
            ex
              paypal: will the customer close his account?
          classification
          entity extraction
            such as names or places
      detecting sequences
        ex
          shopper's sequence of actions in market
            dramatic effect on sales
            ways of enticing customers pick up something
          financial modeling
          intrusion detection
          genetics research
      working with time series
        uses
          sales, economic forecasting
          signal analysis
          astronomy
          epidmiology



## ch19: Ten Great Resources for Data Miners

    society of data miners
      www.socdm.org
    kdnuggets
      kdnuggets.com
      news site
    all analytics
      allanalytics.com
    nytimes
    forbes
      authors
        gil press
        piyanka jain
        naomi robbins
        lisa arthur
    smartdata collective
      curated content
    crisp-dm
    nate silver
      fivethirthyeight.com
    meta's analytics articles
    bit.ly/metaarticles
    gallery of statistics jokes



## ch20: Ten Useful Kinds of Analysis That Complement Data Mining

    business analysis
    conjoint analysis
      think of product manager
        to attract customers
        what are features most appealing?
      role of conjoint analysis
        getting info about consumer preferences
    design of experiments
    marketing mix modeling
      which combination of media provides best value for your needs
      how to allocate spending
    operations research
    reliability analysis
      psychometrics
        consistency in measurement
      engineering
    statistical process control
    social network analysis
    structural equation modeling
      what factors cause consumers to be satisfied
      how to influnece them to improve satisfaction
    web analytics



