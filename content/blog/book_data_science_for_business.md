---
title: "Notes from Book: Data Science for Business"
date: 2017-11-23T17:24:54+03:00
description: ""
tags:
categories: ["datascience", "book_notes"]
type: post
draft: false

---

My reading notes from the book.

<!--more-->

<!-- toc -->

## chapter 01. “Introduction: Data-Analytic Thinking”

	introduction
			past 15 years
					vast amount of data
					increasing interest for extracting useful information
					widest applications
							marketing: targeted marketing, online advertising, recommendations
							crm: analyze customer behavior
									maximize customer value
							finance
									credit scoring
									trading
									fraud detection
									workforce management
			goals
					view business problems from data perspective
					understand principles of extracting useful knowledge
							fundamental structure to data-analytic thinking
							basic principles
							data perspective provides
									structure and principles
									framework to systemetically analyze problems
			terms: data science and data mining
					data science
							a set of fundamental principles
									in extracting knowledge
					data mining
							extraction of knowledge
									via technologies
					more broad: data science
			why to understand data science
					to spot unrealistic assumptions, missing pieces for data mining projects
			book describes
					fundamental data science principles
					show each with one data mining technique
	two case studies
			example: hurricane frances
					wal mart: forecast based on what happened previous hurricane
					why is prediction useful?
							people would buy more bottled water
							local stores properly stocked
					how to discover patterns that are not obvious?
							identify unusual local demand for products
					what happened?
							strawberry pop-tarts increase in sales seven times
							top selling item: beer
			example: predicting customer churn
					MegaTelco: telco firm
					20% of customers leave when contracts expire
					difficult to acquire new customers
					churn
							customers switching from one company to another
					since attracting new customers is expensive
							a lot marketing allocated to prevent churn
					customer retention
							major use of data mining
	Data Science, Engineering, and Data-Driven Decision Making
			data science
					what is
							principles, processes, techniques
							to understand events
							via analysis of data
					ultimate goal
							improve decision making
			data-driven decision-making (ddd)
					basing decisions on analysis of data
					ex
							selecting advertisements based on
									experience
									analysis of data of how consumers react to different ads
					proof for benefits
							erik brynjolfsson from mit
							more data-driven a firm is
									more productive it is
							one standard deviation higher on ddd scale
									4-6 % increase in productivity
							relationship is causal
			2 decision types
					type 1 and 2
							where discoveries need to be made within data
							decisions that repeat at massive scale
					ex: Walmart and MegaTelco
							Walmart: type 1
									discover knowledge to prepare hurricane
							Target market: type 1 (ref: Duhigg, 2012)
									consumers
											inertia in their habits
											new baby -> change in shopping habits
											"when they buy diapers, they buy everything else too"
											birth records public =>
													retailers send special offers to new parents
									how to predict that people expect a baby?
											analyzed historical data 
													customers who later revealed to have been pregnant
											ex
													pregnant mothers change their
															diets, wardrobes, vitamin regimens
							predictive models in general: type 1
									focus on a particular indicator that correlates with a variable
											who will churn
											who will purchase
											who is pregnant
									not testing a simple hypothesis
									data explored
											to discover something useful
							churn example of MegaTelco: type 2
									improve our ability to estimate
											large benefits by applying it to millions of customers
			applications 
					fields
							direct marketing
							online advertising
							credit scoring
							financial trading
							help-desk management
							fraud detection
							search ranking
							product recommendation
					90s
							banking and consumer credit industries
									data-driven fraud control
							telecom
							retail systems
									merchandising decisions
											Harrah's casinos' reward programs
											recommendations of Amazon and Netflix
					now
							advertising
	Data Processing and "Big Data"
			data processing
					relation to data science
							not a subset of data science
							support data science
							more general than data science
					does not involve
							extracting knowledge
							data-driven decision-making
			big data technologies
					such as
							hadoop, hbase, mongodb
					means
							datasets too large for traditional data processing systems
					study by Prasanna Tambe (Tambe 2012)
							big data technologies correlated with productivity growth
							one standard deviation of higher utilization -> 1-3 % higher productivity
	From Big Data 1.0 to Big Data 2.0   
			web 1.0
					goal
							establish a web presence
							build ecommerce capability
							improve efficienty in operations
					general
							build capability to process large data 
									to improve efficiency
					after web 1.0
							rise of voice of individual consumer
			big data 2.0
					what can i do now better?
	Data and Data Science Capability as a Strategic Asset
			key strategic assets
					data
					capability to extract useful knowledge
			for most companies 
					data analytics
							value from existing data
							without regard to appropriate analytical talent
			viewing as assets
					one should invest in them
					we don't have 
							right data
							right talent
					not trivial
			case: Signet Bank 90s
					in 80: transformation in consumer credit
							modeling the probability of default
							credit cards had uniform pricing
					around 90
							do predictive modeling
							offer different terms
									pricing
									credit limits
									low rate transfers
									cash back
									loyalty points
					problem
							no appropriate data to model profitability
					solution
							acquire necessary data at a cost
									learning cost
							different terms offered at random
									charge-off rate went from 2.9% to 6% (losses)
					next
							customer retention
									customer calls for a better offer
									data driven models predict potential profitability of different offers
					Capital One
							2000: 45000 scientific test were carried
			study: Martens and Provost 2011
					does data of bank's consumers improve models for deciding product offers?
					detailed data on customers' transactions improve performance
							more data better performance =>
							banks with bigger data assets => 
									increased adoption of bank's products
									decreased cost of customer acquisition
					Amazon
							value in rankings and recommendations
					Facebook
							data about individuals and their likes
							structure of social network => (Hill, Provost, Volinsky 2006)
									who will buy certain products
	Data-Analytic Thinking
			digital 100 companies (Business Insider 2012): high valuations
					due to primarily data assets
			need for business guys
					managers: oversee analytics teams
					marketers: organize data-driven campaigns
					venture capitalists: invest wisely in businesses with data assets
					strategists: devise plans that exploit data
					ex
							assess wthere a data mining project makes sense
							competitor announces a new data partnership
									when does it put you at a strategic disadvantage
					mckinsey estimates
							talent with data-analytic skills
							2018: 
									shortage of 140-190 K people with deep analytical skills
									1.5 M managers+analysts with data skills (Manyika, 2011)
	Data Mining and Data Science, Revisited
			ex: churn-prediction example
					take data on prior churn
					extract patterns of behavior that are useful
					to predict customers that are more likely to leave
					to design better services
			fundemantal concept: a process with well defined stages
					CRISP-DM: Cross Industry Standard Process for Data Mining
					judgment:
							following a process systematically
							to solve business problems
							by extracting useful knowledge from data
			fundamental concept: finding informative attributes of entities
					judgment:
							finding informative attributes of entities
							by using information technology
							from a large mass of data
					ex: churn
							customer: entity of interest
									described by a number of attributes
											usage, customer service history, other factors
											which one gives information on likelihood of leaving?
							notion: finding variables that correlate with churn
			fundamental concept: overfitting a dataset
					judgment
							you can find something
							but it might not generalize beyond your data
			fundamental concept: context is part of data mining
					judgment
							thinking about the context 
							where the results will be used
							is part of data mining
	## Chemistry Is Not About Test Tubes: Data Science Versus the Work of the Data Scientist
			discussions of data science mention
					analytical skills and techniques
							random forests, support vector machines
					application areas
							recommendation, ad placement optimization
					tools used
							hadoop, spark
			young discipline
					good experts are
							good technicians
	Summary

## Chapter 2. Business Problems and Data Science Solutions

	fundamental concepts
			set of canonical data mining tasks
			data mining process
			supervised versus unsupervised data mining
	data mining is a process
			with well-undestood stages
					involve
							it
									automated discovery
									evaluation of patterns
							creativity
							business knowledge
	From Business Problems to Data Mining Tasks
			decompose a problem into pieces
					each piece matches a known task
			algorithms and types of tasks
					large number of data mining algorithms
					small number of types of tasks algorithms address
			term: individual
					entity about which we have data
					ex
							customer
							business
			project type:
					finding correlation 
							between
									variable describing individual
									other variables
							ex
									leaving customers
									which other variables correlate with it
							example of classification and regression tasks
			tasks of data mining
					classification task
							goal
									estimate the set of classes
									an individual belongs to
							ex
									which customers will respond to a given offer
											will respond
											will not respond
							related task
									scoring or class probability estimation
											probability that individiual belongs to a class
					regression (value estimation)
							goal
									estimate the numerical value
									of some variable for an individual
							ex
									how much will a customer use the service
									predicted: service usage
							comparison with classification
									classification: whether something will happen
									regression: how much something will happen
					similarity matching
							goal
									identify similar individuals
							ex
									find companies similar to the best customers
											based on "firmographic" data
							applications
									product recommendation
					clustering
							goal
									group individuals by their similarity
									not driven by any specific purpose
							ex
									do customers form natural segments?
							applications
									preliminary domain exploration
									input to decision making questions
											what products should we offer?
											how should customer care teams be structured?
					co-occurrence grouping
							names
									association rule discovery
									frequent itemset mining
									market-basket analysis
							goal
									find associations between entities
									based on transactions
							ex
									what items are commony purchased together?
							comparison
									clustering: similarity based on objects' attributes
									co-occurrence: similarity based on their appearing together in transactions
							ex
									supermarket
											ground meat is purchased togther with hot sauce
									recommendation systems
											pairs of books purchased by same people
					profiling
							names
									behavior description
							goal
									characterize typical behavior of an individual
							ex
									what is typical cell phone usage in this segment?
							application
									anomaly detection
											fraud detection
											monitoring intrusion to computer systems
											ex
													determine whether a new card transaction fits that profile
													suspician score -> issue an alarm
					link prediction
							goal
									predict connection between data items
											a link should exist
											strength of link
							ex
									you and karen share 10 friends
											would you like to be karen's friend?
									recommending movies
											graph between customers and movies they rated
											predict links that should exist and be strong
					data reduction
							goal
									compress data
											input: large data
											output: small data that contains much of the important information
							ex
									massive dataset on consumer movie preferences
											reduced to small dataset
											to reveal consumer tastes 
					causal modeling
							goal
									what events influence others
							ex
									targeting advertisements
											observation: targeted consumers purchase more
											question: 
													is this because of advertisement?
													or predictive model identified the right customers?
							how
									randomized controlled experiments
											called: A/B tests
									counterfactual analysis
											what would be the difference between situations
													where the treatment event 
															were to happen
															and were not to happen
									involves assumptions
											ex: placebo effect
	Supervised Versus Unsupervised Methods
			ex: supervised vs. unsupervised classes
					customer population
							do customers fall into different groups?
									no specific target
									=> unsupervised
							find groups with high likelihood of canceling the service
									specific target
									=> supervised
			condition for supervised
					specific target
					there is data on target
							value for target: label
									often: before data mining
											actively labelling data is required
			methods: supervised or not
					supervised methods
							classification
							regression
							causal modeling
					either
							similarity matching
							link prediction
							data reduction
					unsupervised
							clustering
							co-occurrence
							profiling
			type of target in classification and regression
					regression: numerical 
					classification: categorical (often binary)
					ex
							will customer purchase s1 if given incentive I?
									classification with binary target
							which service will customer purchase if given incentive I?
									classification with multi-valued target
							how much will customer use the service?
									regression
					subtleties
							for business applications: numerical prediction better
									ex: churn
											probability that the customer will continue
											still considered as classification
													or: class probability estimation
							in early stages:
									i) decide supervised or unsupervised
									ii) if supervised, define target variable
			process
					model building
							historical data
									x       y   z   class
									14  T   R rejected
									...
							data mining -> model
					model using
							new data
									x       y   z   class
									30  T   R ?
							apply model
							result
									class: accepted
									probability: 0.88
	Data Mining and its Results
			difference 
					between
							mining data
							using results
					results should influnce data mining process
	Data Mining Process
			craft
			CRISP-DM
					business understanding -> data understanding -> data preparation -> modeling -> evaluation -> deployment
			business understanding
			data understanding
					strengths and limits of data
					costs of data
					ex
							fraud detection problems 
									credit card
											transactions have reliable labels
											supervised method
									Medicare
											fraud perpetrators are 
													legitimate users and service providers
													subset of legitimate users
											data has no reliable target variable
											unsupervised methods
									both: fraud, but very distinct problems
			data preparation
					separate book: Pyle 1999
					beware of leaks
							Kaufman et al. 2012
							what is leak
									information appears in historical data
									but is not available at decision time
							ex
									predicting if a web visitor end session
											variable: total number of webpages visited
									predicting if a customer will be a big spender
											known in history: 
													categories of items purchased
													amount of tax paid
											but not known at decision time
			modeling
			evaluation
					common flaw with detection solutions
							such as
									fraud, spam, intrusion monitoring
							too many false alarms
					testing in lab and in business may be different
					in vivo evaluation
							randomly apply model to some customers
							keep a control group
			deployment
	Implications for Managing the Data Science Team
			mistake:
					viewing data mining process as software development cycle
					software development
							milestones are clear
									success is clear
					data mining
							exploratory
									closer to research
									crisp cycle iterates on
											approaches and strategy
											not on software designs
							outcomes less certain
							results can change understanding of the problem
			analytics skills vs. software skills
					software
							writing effcient code from requirements
					analytics
							formulating problems well
							prototyping solutions quickly
							making reasonable assumptions in ill-structured problems
							designing experiments
							analyzing results
	Other Analytics Techniques and Technologies
			main difference
					data mining: focus on automated search for
							knowledge, patterns, regularities
					important: what analytic technique is appropriate for a particular problem
			statistics
					numeric values
							summary statistics
									wrt. distribution of data
					field of study
							contrast
									dm: hypothesis generation
			database querying
					sql
					query by example
					OLAP 
							done in realtime
							unlike ad hoc querying with SQL
									dimensions must be pre-programmed
			data warehousing
					collect data from enterprise
							multiple systems
					integrates records from sales, billing, hr etc.
			regression analysis
					dm: not interested in generalization to population
			machine learning and data mining
					methods for extracting (predictive) models
							developed in several fields
									machine learning
											subfield of artificial intelligence
													concerned: improving knowledge of an agent in response to his experience
									applied statistics
									pattern recognition
					data mining (KDD: knowledge discovery and data mining)
							started from machine learning
									both: try to find useful patterns
									techniques are shared
							kdd a subfield of ml
							more concerned with entire process:
									data preparation, evaluation
	Answering Business Questions with These Techniques
			Who are the most profitable customers?
					if profitable is in existing data
							just a database query
			Is there really a difference between the profitable customers and the average customer?
					about a conjecture or hypothesis
							there is a difference
					method: statistical hypothesis testing
			But who really are these customers? Can I characterize them?
					common features of them
					from database: using database querying
					deeper analysis
							what features differentiate profitable customers from others
			Will some particular new customer be profitable? How much revenue should I expect this customer to generate?
					examine historical data
					produce predictive model of profitability

## Chapter 3. Introduction to Predictive Modeling: From Correlation to Supervised Segmentation

	Fundamental concepts: 
			Identifying informative attributes; 
			Segmenting data by progressive attribute selection.
	Exemplary techniques: 
			Finding correlations; 
			Attribute/variable selection; 
			Tree induction.
	predictive modeling
			as supervised segmentation
					how segment population wrt sth that we predict
			target in predictions
					something we want to avoid
							ex
									which customers are likely to leave
									which accounts have been defrauded
									which customers are likely not to pay off
									which web pages contain objectionable content
					positive target
							ex
									which consumers are likley to respond to an ad or offer
									which web pages are appropriate for a search query
			fundamental idea of dm
					finding informative variables or attributes of entities described by data
					meaning of "informative"
							information: quantity that reduces uncertainty about something
					supervised dm:
							specific target exists
							the target quantity is unknown
									customer will churn?
									accounts has been defrauded?
							finding informative attributes
									is there other variables that reduces uncertainty about value of the target?
									find knowable attributes
											that correlate with target of interest
									basis for tree induction
					terminology 
							ex
									name,balance,age,employed,write-off
									Ali,115,40,no,no
							feature vector: <Ali,115,40,no>
							class label (value of target attribute): no
							attributes: name,balance,age,employed,write-off
							target attribute: write-off
	Models, Induction, and Prediction
			model
					simplified representation of reality to serve a purpose
					simplified
							on assumptions
									what is important
			predictive model
					formula to estimate unknown value of interest: target
					formula can be
							mathematical
							logical rule
					terminology: prediction
							data science: to estimate an unknown value
					contrast to descriptive modeling
							purpose: gain insight into process
							ex: churn
									what do customers typically look like
							criterion: intelligibility
									less accurate model better if easier to understand
									pm: predictive performance
					terminology:
							supervised learning
									model creation
									model describes a relationship between
											set of selected variables (attributes or features)
											predefined variable called target
									model estimates value of target as a function of features
											possibly a probabilistic function
									instance or example
											a fact or a data point
													ex: a historical customer given credit
											usually a row in database
											described by a set of attributes
													fields, columns, variables, features
											also called: feature vector
													fixed length ordered collection of feature values
			many names for same things
					principles studied in different fields
					dataset
							also: 
									table of database
									worksheet of spreadsheet
							contains
									a set of examples or instances
									instance
											also: 
													row of database table
													case in statistics
							features
									also: 
											table columns
											independent variables (stats)
											predictors: input attributes (stats)
											explanatory variable (operations research)
							target variable
									also
											dependent variable (stats)
							selec
			model induction
					creation of models from data
					term: from philosophy
					contrast: deduction
							starts with general rules and specific facts
							creates other specific facts
					input data for induction algorithm
							used for inducing model
							called: training data
									also: labeled data
											because value of target is known
					ex: churn problem
							build a supervised segmentation model
									that divides sample into segments
	Supervised Segmentation
			human understandable set of segmentation patterns
					ex:
							middle aged professionals who reside in NYC have a churn rate of 5%
									predicted target value: 5%
			fundamental concept:
					how to judge whether a variable contains important information about target?
					how much?
			Selecting informative attributes
					ex: stick people
							attributes:
									head shape: square, circular
									body shape: rectangular, oval
									body color: gray, white
							target variable:
									write-of: yes, no
					goal:
							resulting groups to be as pure as possible
									homogeneous wrt target variable
									every member of group has same value for target
					complications
							solution
									formula based on purity measure
					splitting criteria
							information gain
									most common
									based on a purity measure: entropy 
									invented by Claude Shannon 1948
									entropy
											measure of disorder
											consider
													a set of properties of members of the set
													each member has one property
													in supervised segmentation:
															member properties = values of target variable
															disorder = how mixed (impure) the segment is wrt properties
															_ref: dscp20150626.1
											entropy = - p_1 log (p_1) - p_2 log (p_2) - ...
													p_i: probability of property i within set
															p_i = 1: all members have property i
													entropy function of two class set
															_fig: 3.3
																	if pure => 0
																	if randomly mixed => 1
					how informative is an attribute wrt target
							how much gain in information it gives us about value of target
							an attributes 
									segments a set of instances
									into several subsets
							contrast: entropy
									how impure one individual subset is
							define: information gain (IG)
									using entropy
									to measure
											how much an attribute improves entropy
									measures
											change in entropy
											due to new information 
									IG(parent, children) = entropy(parent) -
											(p(c_1) x entropy(c_1) + ...)
									entropy(c_i)
											weighted by proportion of instances belonging to that child
									ex
											attribute has k different values
											original set: parent set
											result of splitting on k values: children sets
											_fig: 3.4
							what if attribute is numeric 
									discretize by choosing split points
					regression problems
							information gain is not right measure
									because ig is based on properties in segments
							measure of impurity: variance
									set pure when variance is zero
											all values in set are same
			Example: Attribute Selection with Information Gain
					goal
							which attribute is most informative wrt estimating value of target
							rank a set of attributes by their informativeness
					problem
							which attribute is most useful for distinguishing edible mushrooms from poisonous ones?
					_fig: 3.7
			Supervised Segmentation with Tree-Structured Models
					goal
							select multiple attributes
							how to put them together?
					multivariate (multiple attribute)
					classification tree
							nodes
									interior
											contains a test of an attribute
									terminal or leaf
											= segment
											attributes and values along the path = characteristics of the segment
							branches
									distinct value of attribute
							how to build it?
									divide-and-conquer approach
											start with whole dataset
											apply variable selection
											choose the split with most information gain
					probability estimation tree
							to predict the probability of membership in the class
									ex: probability of churn or write-off
											not the class itself
	Visualizing Segmentations
			decision lines and hyperplanes
					decision lines
							also:
									decision surfaces
									decision boundaries
							lines separating the regions
					each node: 
							an (n-1) dimensional hyperplane decision boundary on instance space
					_fig 3.15
	Trees as Sets of Rules
			rule set
					IF (Balance < 50K) AND (Age < 50) THEN Class=Write-off
					...
	Probability Estimation
			ex: churn prediction
					rank prospects by probability of leaving
					high budget to instances with high expected loss
			ex: credit default
					most instances will "not write-off"
					most leafs in tree: not write-off
					_fig 3.15
			how
					frequency based estimate of class membership probability
							we have frequencies of each property in each segment
							use them as class probability estimate
							overfitting in small samples
									if a leaf has a single instance => 100%
							smoothed version
									known as: Laplace correction
	Example: Addressing the Churn Problem with Tree Induction
			how good are each variable indivdually?
					this is different from multivariate classification tree
							depends on previous nodes


