---
title: "Book Notes: Developing Analytic Talent - Vincent Granville"
date: 2017-11-23T17:26:18+03:00
draft: false
description: ""
tags:
categories: ["datascience", "book_notes"]
type: post
draft: false

---

My reading notes from the book

<!--more-->

<!-- toc -->

## ch 01 - What Is Data Science?

	what is not data science?
	fake data science
			repackaging old material as data science
			more than
					mapreduce, hadoop, python, nosql
					you can be data scientist
							without these skills
			you need
					business acumen
					real big data expertise: 50 M row data
					ability to sense the data
					distrust of models
					knowledge of curse of big data
					communicate well
					assess lift on the salary
					identify a simple solution
					passion for analytics
					experience with success stories
					data architecture 
					data cleaning
					computational complexity
					knowledge of algorithms
			generalist in
					business analysis
					statistics
					computer science
			expert in such as:
					robustness
					design of experiments
					algorithm complexity
					dashboards
			examples
					statistics courses
					big data 2005
			modern data
					velocity
							real time, fast
					variety
							structure, unstructured
					volume
			face of the new university
					university costs
							2000 $ for professor
							50 K revenue
							why?
									overhead
									alternative training much cheaper
	data scientist
			data scientist vs data engineer
					etl vs dad
							etl: for data engineers, dba
							dad (discover/access/distill): for data scientists
					dad
							discover
									identify good data sources
							access
									sometimes via api, crawler, internet download
							distill
									extract information that leads to decisions
									exploring data
									cleaning data
									refining data by summarization
									statistical analysis
									presenting results
					unique core
							advanced visualizations
							analytics as a service and api
							clustering and taxonomy creating
							correlation
							features of database
							fast feature selection
							mapreduce
							internet topology
							keyword correlations
							linear regression
							confidence intervals
							predictive power of a feature
							statistical modeling without models
			data scientist vs. statistician
					difference
							involves implementing algorithms to process data automatically
									analyzing nasa pictures
									automated bidding systems
									automated piloting
									book and friend recommendations on amazon, facebook
									client customized pricing in hotels
									computational chemistry to simulate
									early detection of an epidemic
									estimating the value of houses
									high frequency trading
									matching a google ad with a user to maximize conversion
									relevant results of searches
									scoring credit card transactions (fraud)
									tax fraud detection
									weather forecastss
			data scientist vs. business analyst
					business analysts
							database design
							roi assesment
							project management
					data scientists
							can help them
									by automating analysts's tasks
	Data Science Applications in 13 Real-World Scenarios
			two types of problems
					internal
							bad data, inappropriate techniques
							not business problems
					applied business problems
							ex: fraud, causal study
			scenarios
					DUI Arrests Decrease After End of State Monopoly on Liquor Sales
							law: allow grocery stores to sell liquor
							why did arrests decline?
							flow
									list of possible explanations
									plan to rule out some
									attach weights to them
					data science and intution
							most decisions in management
							twin data points
									observations that are almost identical
									it is the norm
									for all data sets
							ex: stars in a galaxy
									intuition: there is a force to cluster in pairs
									technical note: 
											how probability computed
													init
															say a sky image 10x10 cm
															n= 500 stars
															twin stars if 1 mm away
													poisson process: 
															L: number of points per square mm: = 500/(100x100) = 0.05
															P(a star has a neighbor in 1 mm) = 0.145
															being a binary star:
																	bernoulli variable of p = 0.145
											monte carlo simulation
													complexity
															O(n^2) if distance computed for each star
															O(n) if data stored as a grid of 100x100. check stars in 8 surrounding cells
					data glitch turns data into gibberish
							ex
									access: date values: sometimes integer, sometimes mmddyy
									wells fargo: each server had its own web log. blending was not done
									ebay: removed special characters from German
											lookup table of foreign kewyords built
									click fraud detection
					regression in unusual spaces
					... @todo
	Data Science History, Pioneers, and Modern Trends
			declining trends
					statisticians
					computer scientists
			increasing trends
					data analysts
					data scientists
			Statistics Will Experience a Renaissance
					data analysts?
							junior title
							BS or BA degree
							reports and sql queries
					statistics
							more theoretical
							use models
							MS or PhD degree
			History and Pioneers 
					popular keywords in data science
							1988
									artificial intelligence
									computational statistics
									data analysis
									pattern recognition
									rule systems
							1995
									web analytics
									machine learning
									business intelligence
									data mining
									roi
									distributed architecture
									data arcihetcture
									quant
									decision science
									knowledge management
									information science
							2003
									business analytics
									text mining
									unstructured data
									semantic web
									nlp
									kpi
									predictive modeling
									cloud computing
									lift, yield
									nosql
									business intelligence bi
									real time analytics
									collaborative filtering
									recommendation engines
									mobile analytics
							2012
									data science
									big data
									analytics
									saas
									on demand analytics
									digital analytics
									newsql
									hadoop
									in memory analytics
									sensor data
									healthcare analytics
									utilities analytics
									data governance
									in column databases
							2022
									data engineering
									analytics engineering
									data management
									data shaping
									art of optimization
									optimization science
									optimization engineering
									business optimization
									data intelligence
			Modern Trends
					current issues
							integrating analytics in big distributed databases
									data architects and data scientists communicate with each other
									moving away frfom data-to-analytics vs. analytics-to-data
									analtics-to-data
											performing analytics inside database
											rather than downloading data and then processing it
											sometimes called in-memory analytics
											requires new languages rather than sql
													pivotal: FastSQL
															write sql, run hadoop
							automated machine-to-machine communications in real time
									ex: 
											high-frequency trading
											massive bidding systems
													ebay updating keyword bids for 10 M keywords on Google PPC 
															in real time
															based on keywords' performance or analytic scoring
									via
											APIs
											AaaS (on-demand analytics as a service)
							variety of data
									unstructured: twitter
									structured: sensor
					characteristics
							in-memory analytics
							hadoop
							nosql, newsql, graph
							python, r
							data integration: unstructured
							visualization
							AaaS
							text categorization/tagging and taxonomies
					casualties
							Perl: replaced by python
							maybe Excel
			Recent Q&A
					TDWI group on LinkedIn (The Data Warehousing Institute)
					optimizing joins in sql or moving away from sql
							hash table joins
							shows the conflicts between data scientists, architects and business analysts

## ch 03

	sample proposals
			proposal 1
					click fraud detection
					proof of concept (5 days)
							process test data set
									recent click data
											keyword
											referral id
											feed id
											affiliate id
											cpc
											session id
											user agent
											time
							identification of fraud cases
							comparing my scores with your internal scores
							explain discrepancies
							cost: 4K $
					creation of rule system
							creation of 4 rules/wk for 4 weeks
							cost: 200 $ / rule
					creation of scoring system
					... @todo
			proposal 2
					web analytics, keyword bidding, a/b testing, keyword scoring
					1. purpose
							predict conversion rates for keywords with little data in ppc
							use
									text mining
									rule engine
									predictive modeling
							build
									keyword score for each client
							implement
									keyword bidding algorithm based on keywword scores
					2. methodology
							traninig set
									scores based on 
											250 K keywords from one client
									for each 
											bid keyword / match type / source
											fields
													clicks
													conversions
													conversion type
													revenue per conversion
											for 8 weeks

## ch 06 Data Science Application Case Studies

	stock market
			pattern to boost return
					@todo
	fraud detection
			click fraud
					generated by ad network affiliates
					types
							clicking competitors' ads
							to increase income of affiliate
									by botnets, paid people
					poor traffic
							invalid clicks
									non-billable traffic
									accidental fraud
											robot run by spammers
									political activists
									fired employees
	digital analytics
			borders between data science and other domains
					esp. business intelligence - data analysts
					small companies: data scientist does everything
			online advertising
					how many impressions to purchase to achive the goals
					solution
							Reach = Unique Users – SUM{ U_k * ( 1 – P )^k}
									U_k: # users turning k pages in time period
									P: ratio of purchased impressions by total pages
			email marketing
					improve open rates by 300 %
					how?
							remove subscribers that didn't open in last 8 deployments
									improved spam score
							segment target members
									don't send uk conference to asia
							stop sending messages with low openrates
							get member information for better segments
									profession, industry, location...
							grow subscriber list
									offering free ebook
									linkedin group members to subscribe
							a/b testing in real time
									try 3 subject lines with 3K subs
									then use the best one
							identify patterns in subject lines
			optimize keyword advertising campaigns
					example application
							identify 10 top keywords. seed
							create large list of bid keywords
							grow further
							find keywords that are on your web, competitors site
			automated news feed optimization
					producing great content
			intelligence with bitly
			measuring return on twitter hashtags
			improving google search
			improving relevancy algorithms
			ad rotation problem
	Miscellaneous
			sales forecasts with simpler models
			detection of healthcare fraud
			attribution modeling
			forecasting meteorite hits
			data collection at trailhead parking lots
			other
					oil 
							drill wells
					credit scoring

## ch 07 - Launching Your New Data Science Career

	Job interview questions
			open ended
			experience
			technical
			general
			data science projects
	Testing you own visual and analytic thinking
			detecting patterns with naked eye
					scatterplots
			identifying aberrations
			misleading time series and random walks
	From Statistician to Data Scientist
			why statisticians are not involved in big data
			Data scientists are statistical practitioners
			who should teach statistics to data scientists
			hiring issues
	Taxonomy of a data scientist
			skills
					data mining
					machine learning
					analytics

