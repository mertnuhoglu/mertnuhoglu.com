---
title: "Book Notes: Implementing Analytics - Nauman Sheikh"
date: 2017-11-23T19:05:21+03:00
draft: false
description: ""
tags:
categories: ["datascience", "book_notes"]
type: post

---

My reading notes from the book

<!--more-->

<!-- toc -->

## ch01 Defining Analytics

    Challenge of Definition
      dictionaries
        logical analysis method
        logic with analysis
        turning data into insights into action
      definition 1: business value perspective
        three variations of value
          present
          past
          future
        data created
          in operational system
      definition 2: techincal implementation perspective
        four data analysis methods
          forecasting
          descriptive (clustering, association rules ...)
          predictive analytics (classification, regression, text mining)
          decision optimization
    Analytics Techniques
      4 techniques
        forecasting
        descriptive analytics (primarily clustering)
        predictive analytics (primarily classification, also regression and text mining)
        decision optimization
      algorithm vs. analytics model
        each technique implemented
          by an algorithm
        algorithm: like an instrument
          tune you produce: analytics model
      forecasting
        related terms
          time series analysis
          sequence analysis
      descriptive analytics
        clustering
          grouping data points
      predictive analytics
        based on
          how similar observations were classified in past
        also known
          directed analytics
            training data shows what to predict
        prediction
          what class a new observation belongs to
        ex
          probability that a coupon offer will be redeemed leading to a sale?
          probability a credit card will default
          probability that a product has a defect leading to a warranty claim
        how it works
          training data:
            with known outcomes
          algorithm learns
            from pattern of known outcomes
            what variables influence the likelihood of a certain outcome
              discriminatory power of a variable
        prediction vs. forecasting
          forecasting
            time series
            new value may not have occurred in the past
            no likelihood value
          regression
            prediction
            historical value musth ave occurred
          ex: weather
            forecasting:
              what is high temperature tomorrow?
            prediction
              likelihood that it will be 72?
        prediction methods
          regression
            classic
          data mining
            decision trees
            neural networks
            naive bayes
          decision trees
            simplest
            ex: fig 1.6 training data set
        text mining
          predict categories of text
      decision optimization

## ch02 Information Continuum


## ch03 Using Analytics

    Healthcare
      Emergency Room Visit
        probability that a patient will be back in ER in next 3 months?
          analytics solution
            predictive model
            predictive variable:
              ER_Return
            historical ER data
              3-5 years
              paitent visit
              age
              gender
              profession
              marital
              diagnosis
              procedure 1
              procedure 2
              date
              vitals 1-5
              previous diagnosis
              previous precedure
              current medication
              last visit to ER
              insurance coverage
      Patients with the Same Disease
        identify common patterns among patients with same disease
        analytics solution
          goal: clustering
          variables
            age, gender, economic status, zip code, children?, pets?, medical conditions, vitals?
    CRM
      customer segmentation
        build customer segments based on the similarity of their profile
        analytics solution
          one record per customer
          variables
            age, gender, children, income, luxury car, zip code, number of products purchased, avg products, distance from store, online shopper
      propensity to buy
        track how many coupons are utilized? overall benefit of the campaign?
        problem: probability that a customer buys the product upon receiving coupon
        analytics solution
          predictive problem
          predictive variable:
            Uses_Coupon
          age, demographics, purchases, departments and products purchased, coupon date, type, coupon delivery method
    Human Resources
      not as widespread
      ex
        predict employees potentially leaving
      employee attrition
        probability a new employee will leave in first 3 months
        analytics solution
          predictive variable
            Left_Company
          one record per employee
          variables
            employee personal profile, education, interview process, referring firm, last job details, reasons for leaving last job, interest level, hiring manager, hiring department, new compensation, old compensation
          tricky:
            interest leven in new position
      resume matching
        resume run through text mining
          prediction as to the class of resume
        problem: probability that this resume is from a good java programmer?
        analytics solution
          existing known resumes
            should be labeled
            business analyst, database developer etc.
    consumer risk
      mature adoption in banking industry
      products:
        auto loans, credit cards, personal loans, mortgages
      borrower default
        probability that customer default on loan in next 12 months
        analytics solution
          predictive variable
            Defaulted
          one record per loan account
          variables
            customer personal profile, demographic, type of account, loan disbursed amount, term of loan, missed payments, installment amount, credit history from credit bureau
    Insurance
      customer fills an application, model calculates rate
      Probability of a claim
        probability that policiy will incur a claim within 3 years
        analytics solution
          preditive variable
            Incurs_Claim
          one record per insurance policy
          variables
            policiyholder personal profile, policiy type, open date, maturity date, premium amount, payout amount, sales representative, commission percentage
    Telecommunication
      Call Usage Patterns
        build clusters of customers
        analytics solution
          one record per customer
          variables
            calls, sms messages, data utilization, bill payment, demographics, legnth of relationship
    Higher Education
      problem: will the student offered admission, will accept and join the school?
      admission and acceptance
        probability that a student will accept admission
        analytics solution
          predictive variable
            Accepts
          one record per student application
          variables
            sat, essay score, interview score, economic background, ethnicity, personal profile, high school, school distance, siblings, financial aid requested, faculty applied
    Manuafacturing
      to manage supply chain
      driving forces
        3d printing => new designs
        global supply chains => large choice
        volatiliy in pricing => procurement and storage very complex
        customization => adjustments on short notice
      warranty claims
      Predicting Warranty Claims
        probability that next product will lead to warranty claim
        analytics solution
          predictive variable
            Warranty_Claim_Paid
          data grain: product level
            related to
              product specs
              production schedule
              employees
              customer details
      Analyzing Warranty Claims
        clustering similar claims to break down the defect problem into chunks
        analytics solution
          one record per claim
          variables
            product details
            product specs
            customer details
            production schedules
            line workers' details
            sales channel details
            defect details
    Energy and Utilities
      after deregulation: power generation is separate from distribution
        customer care and billing done by energy services
      new challenge
        analytics solution
          weather forecasting
          correlate weather to load
            predictive model
          generation company: probability of firing up power generation plants
          services company: predict additional customers to come online at peak load hits
            clustering models to group their customers
          decision optimization and pricing algorithms to max services companies
    Fraud Detection
      classification and clustering
      benefits fraud
        goverment benefits
        frauds:
          payment transactions 
            has two children but receives for four
          citizien is not eligible
            misrepresenting his income
        probability that an application is fraudelent
        analytics solution
          predictive variable
            Is_fraudelent
          one record per application
          variables
            similar to loan application
      Credit Card Fraud
        transaction that violates expected behavior
        analytics solution
          clustering problem
          clusters of customers based on their behavior
          grain of data
            customer level
            all purchases on card aggregated and related to customer record
          variables
            average spending on gas, shopping, dining out, travel
            full payment, min payment
            percentage of limit utilized
            geography of spending
          clusters
            their values are thresholds
            breaking threshold => fraud alert
    Patterns of Problems
      prediction problem
        break down problem into 1 or 0
        event occurred or not
      
## ch04 Performance Variables and Model Development

    Performance Variables
      Terminology
        columns or data fields
        variables
        performance variables
          aggregate or summary variables
        input variables
          input to analytics algorithm
        characteristics
          input variables with their relative weights and probabilities in model
      What are performance variables?
        Reasons for creating performance variables
          ex:
            ABC sells computers online
            data fields
              entity | data fields
              customer | name, address, credit score ...
              product | product id, type, class, ...
              sales channel | channel id, category, partner ...
              sales transaction | transaction id, code, timestamp, channel ...
            issues
              most data fields empty
              if filled out, with quality issues
              lots of fields have same values
            assume
              # fields: 70
              mostly with null: 23
              good data: 24
              input variables: 24
            second test
              are these fields filled only for customers who bought?
                if so, not useful
            how much data is enough?
              40% of all transactions should be available
                90% for training
                10% for testing
        what if no pattern is available?
          use performance variables
            aggregate variables built using historical data
            same level of grain as input variables
          ex: performance variables
            purchased_in_last_12_mo
            no_of_visits_online
            no_of_customer_service_contacts
            1st_ever_purchase_date
            last_purchase_date
          define new performance variables
        benefit of using performance variables
          as business evolves
            remove some input variables
            add newer performance variables
        creating performance variables
          grain
            is the level of detail at which analytics algorithm will work
          range
            continuous variables are not good
          spread
            how the population is broken out
            ex
              predict likelihood of an insurance policy to redeem a claim
              performance variable
                customer_profit_lifetodate_amount
                value range: -1200-7300
            frequency might be 1-2 for most values
              might be skewed
      Designing Performance Variables
        discrete vs. continuous
          how to convert continuou into discrete?
            ex
              how many buckets to build?
              their ranges?
            best:
              uniform frequencies in each bucket
        nominal vs. ordinal
        atomic vs. aggregate
      Working Example
        performance variables:
          popular_products_purchased
          multiple per customer record
            not at same grain level
          thus divide it:
            popular_products_purchased_1
            popular_products_purchased_2
            popular_products_purchased_3
    Model Development
      What is a Model
        difference: model vs. algorithm
          algorithm
            takes input data
            to produce model
          model
            takes input data
            produces an output (prediction, forecast etc.)
      in Predictive Modeling
        ex: loan application. produce risk score
          ex: characteristic: age
            input variable: age_range_code
            max. weight: 200
            ex:
              value | weight
              < 17	| rejected
              18-24 | 100
              25-45 | 150
          ex: characteristic: previous loan status
            input variable: 12_month_previous_loan_history_status
            ex:
              value | weight
              one or more loand and uptodate payments | 200
              no loans before | -50
              one loan, two payments late | -100
          total risk max: 1000
          two variables: 450 / 1000. 45%
        input variables converted into characteristics to form the model
          relative weights/importance of variables emerge
        ex: Risk Scorecard
          characteristics | % weight | max score
          age | 20% | 200
          previous loans | 25% | 250
          income | 15% | 150
      in Descriptive Modeling
        model built. you have:
          number of clusters
          cluster affinity (closeness)
          cluster characteristics:
            cluster name/id
            input variables and their range of values
            probabilities and correlations of variables in each cluster
      Model Validation and Tuning
        essential bc of blackbox nature of ml algorithms
        predictive model validation
          low false-positives
            error rate of model's predictions
          if probabilistic output
            ex
              shipping company
              predict which shipments will get delayed
              historic data
                pred variable: delayed or not
              confusion matrix
                  actual shipments on time | actual shipments delayed
                predicted on time | 7670 | 170
                predicted delayed | 940 | 1220
  Challenger: A Culture of Constant Innovation

## ch05 Automated Decisions and Business Innovation

    Automated Decisions
      school of thought
        empower data scientists
          give them all models
          let them find out 
        democratization of analytics
          not limited to problems where data scientists are available
          required skills
            data and analytics
            very rare
          automated decision approach
            converting subjectivity 
              into objective transparent set of rules
    Decision Strategy
      decision strategy
        most important part of the book
        nothing matters
          if decision strategy is not properly designed
        analytics has gray area
          not all scenarios 
            can be accomodated
            have enough training data
          scenarios can have conflicting outputs
          change by conditions
            assumes business as usual
        Nassim Taleb
          Fooled By Randomness
          The Black Swan
          weakness of models 
            when surprising event occurs
          surprise events
            not addressed by historical data
          trick
            find balance in decision strategies
            limiting damage from automated decisions
        morgage crisis of 2008
          risk models dubbed risky customers as subprime
          decision strategy was agressive
            morgtgage backed securities risk models
              called them AAA
              didn't factor performance variables
        understanding models
          far more difficult for managers
            vs. managing the decision strategy
      Business Rules in Business Operations
        business rules
          have decision variables (DVs)
            to complete a transaction
            ex
              offer a discount to a good customer
                what is good?
              don't lend to a customer with poor credit
                what is poor?
              reroute a package using a premium service if can delay
                what is "can"?
              stop suspicious money laundering transfer
                what is suspicious?
              hire most suitable candidate
                what is suitable?
          two categories of rules
            expert
            quantitative
        Expert Business Rules
          devised by expert people
          they are
            policiy designers, mentors, to get advice employees, know how to handle things
          business as usual rules (80% of normal business) are 
            either baked into operational systems
            or employees are well trained to address them
        Quantitative Business Rules
          based on hard numbers and absolute values
          ex
            good customer
              has been a customer >1 year
              purchased >300 $
            suspicious money transfer
              > 7500 $
              if customer has account < 200 $
      Decision Automation and Business Rules
        analytics models -> new business rules
        document the process
          describe model
            its output
          context of decision
      Joint Business and Analytics Sessions
        business input
          reasons
            1. they understand their business
            2. their buy-in is needed
      Examples of Decision Strategy
        Retail Bank
          car loan product
          probability that loan will not be paid
            bank does:
              1. historical data of load data. identify loans paid and defaulted
              2. predictive load default model
              3. identify patterns that differentiate
              4. test model
              5. tune model
            decision variables
              probability of deafult
              loan amount 
              down payment
            decision strategy
              IF any of the Policy Variables are TRUE
              THEN Reject
              ELSE
                IF the Probability of Default > 0.62 (62%)
                THEN Reject
                ELSE
                  IF Probability of Default is between 0.28 and 0.62
                  THEN IF Loan Amount Requested <= 10,000
                    THEN Approve with 25% down payment 
                    ELSE (i.e. loan amount requested is > 10,000) Request for a Co-Signer
                  ELSE Approve the loan (i.e. probability of default is < 28%)
            performance of the model
              measured using results of decision strategies
            decision variables and cutoffs
              DVs
                probability of default
                  from risk predictive model
                loan amount
                  part of business transaction
        Insurance Claims
          decision strategy
            fig 5.2
        Decision Strategy in Descriptive Models
    Decision Automation and Intelligent Systems
      Learning versus Applying
        purpose of analytics systems
          analyze data
          learn from analysis
          use that knowledge
          insight to optimize business operations
        optimize business operations
          means
            operational system changes
        two dimensions of decision automation
          learning
            data warehous, analysis, analytics models
          application of that learning on business operatinos
            scope: decision strategies 
        school of thought: active data warehousing
          do decision making in data warehouse
            since all relevant data is available
          requires
            dw connected to operational system
      Strategy Integration Methods
        problem
          how to add decision strategy into operational system?
        ETL to the Rescue
          encompasses
            all aspects of data in motion
              single record, large data set, messaging, files, real time, batch
          ETL: glue that holds entire analytics solution
          integration
            ETL layer
              receives real time event from operational system
              prepares input record around event
              invokes analytics model
              get output from analytics model
              pass it to decision strategy
              reach a decision
              return decision back into operational system
    Strategy Evaluation
      evaluation of decision strategies
        validating automated decision making
        testing alternate approach
      Retrospective Processing
        use historical data
          they have known outcome
        compare them with strategy
      Reprocessing
        run event through existing decision process
        randomly select 10% of transactions
          run through new strategy
          save results
        after some time
          when benefit of decision is evident and measurable
          compare them
  Challenger Strategies

## ch06: Governance Monitoring and Tuning of Analytics Solutions

    Analytics and Automated Decisions
      The Risk of Automated Decisions 
        incorrect decisions carried out automatically
        inaction leading to lost opportunity
        wasted resources
      Monitoring Layer
    Audit and Control Framework
      automated decisions
        2 parts
          analytics
          strategy
        information
          analytics model name / id, type, version
          decision strategy name / id, version
          input values
          output
          decision path
          input date/time, source system, id of input, output date
        real data
          still in operational system
      Organization and Process
        audit and control organization
        data moved into audit datamart
      Audit Datamart
        fig 6.1
        tables
          decision summary fact table
            records decisions
            grain: decision level
          decision code
            decision that strategy eventually  takes
            3 typical decision codes
              accept
              reject
              review
            predictive model
              assigns probability of default
            strategy
              looks at overall situation of applicant, loan offer, product specifics, liquidity, lender's overall strategy
          threshold table
            wrt. automated decisions
            threshold on certain decision code
            allow certain values of decisions
            breaches get recorded in alerts table
          data warehousing best practices
            use of ETL 
              to load data
            regularly scheduled data feeds
            data quality controls
            etc.
        Unique Features of Audit Datamart
          schedule shared
          data discrepancy go to investigation
          changes to datamart signed off by auditors
          separate logs for access and running queries
          tight security controls
          application for threshold table
          alerts table runs nightly
      Control Definition
        Best Practice Controls
        Expert Controls
        Analytical Controls
      Reporting and Action

## ch07: Analytics Adoption Roadmap

    Learning from Success of DW
      main
        how to convert into an analytics driven enterprise
          penetrate all aspects of business
          top-down
            from top executives
          bottom up
            our proposal
      Simplification
        original definition of DW (Bill Inmon 1992)
          reporting strains operational systems
          there is no single version of truth integrated
          solution
            take all reportable data
            move into dw
      Quick Results
        adoption:
          first top-down approach
          large projects
        problem: analysis paralysis
          years to integrate with no value
        then: datamart based approach
          functional use facing databases
            integrating all relevant data
            for that function's reporting needs
          dw was built datamart after datamart
      Evangelize
        no need to convince management this is important
        the more they see, the more they ask for
      Efficient Data Acquisition
        capability in handling 
          various forms of data
          at different schedules
          called: ETL
        vast field: ETL
          involves all aspects
          small industry: data integration
          integration 
            with all operational systems
            mechanism for receiving and sending data
      Holistic View
        business requirements
          come from users of datamarts
        dw teams has business analysts
          that has complete functional and data perspective
        business users trust dw business specialists
      Data Management
        in addition to infrastructure
          data dictionary
          data model
          data lineage
            how did data reach reporting display
          data quality controls
          data dependencies
          transformation business rules
          other metadata
      dw is prerequisite for analytics
      information continuum
        first levels: dw
        analytics on top of dw
    The Pilot
      Business Problem
        constraint of analytics projects
          business does not understand
            what they want
          IT doesn't know
            what to build
      Management Attention and Champion
        don't pick a problem
          for which relevant data is not in dw
      The Project
        deliverables:
          problem statement
          candidate variables as input to model
          mapping of input variables to dw data
          design of specialized analytics datamart
          design of ETL: dw > transform > analytics datamart
          integrate analytics datamart with analytics software
          identify large data sample for pilot
          load analytics datamart
          separate 90% for training, 10% for testing
          run build training models
          validate results, add more variables, repeat to improve
          feed new data to model, return output to business for actions
        tendency to get expensive software
          resist by R and SQL
      Results, Roadshow, and Case for Wider Adoption

## Analytics Organization and Architecture

    Organizational structure
      bicc: business intelligence competency center
        responsible for
          implementation (incl dw)
          analytics
          decision strategies
          across Information Continuum
      BICC Organization Chart
        ETL
          design and architecture
          scheduling and maintenance
          ETL development
        Data Architecture
          data modeling
          data integration
          metadata, tool, repository
        Business analysis
          data analysis
          analytics analysis
          requirements analysis
        Analytics
          analitics modeling
          analytics implementation
          decision strategies
        Information delivery
          reporting development
          analytical application development
          operational integration development
        Additional support teams
          Database administration
          technology infrastructure
          project management
          quality assurance and data governance
          information security
          compliance, audit, control
      Roles and Responsibilities
        ETL
        Data Architecture
        Business Analysts
        Analytics 
        Information Delivery
      Skills Summary
        Analytics Analyst
        Analytics Architect
        Analytics Specialist
    Technical Components in Analytics Solutions 
      Analytics Datamart
        Base Analytics Data
        Performance Variables
        Model and Characteristics
        Model Execution, Audit, and Control

## Referred Books

    Kimball
    Innon
    Han

