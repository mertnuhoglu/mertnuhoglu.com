---
title: "Notes From Book: Big Data Revolution - Rob Thomas"
date: 2017-11-23T15:52:22+03:00
description: ""
tags:
categories: ["datascience", "book_notes"]
type: post
draft: false

---

My reading notes from the book.

<!--more-->

<!-- toc -->

## Prologue

    Berkeley, 1930s
        Dantzig
            late on exam
            3 questions on board
            solved one of them
            accidentally developed simplex algorithm
    Pattern Recognition
        Gladwell - Outliers
            patterns of successful people
            environment directly correlates to success
                month born
                upbringings
                culture raised
        Nelson Peltz
            private equity and activist investor
                most focus on financial engineering
                very few have insight of business operations
                    requires understanding patterns
            focused in two sectors
                consumer packaged goods
                food
            sample of patterns he looks for:
                ratios: percent of sales spent on marketing
                overhead: growth in overhead vs. growth in sales
                rebates and allowances: deals and allowances paid to retailers
                brand: 
                    more efficient to revitalize previously great brands
            Heinz 
                selling, general, administrative (sg&a) expenses
                    higher than others
                advertising cost: out of line
                rebates and allowances: much higher
            Wendy's
                pattern of underperformance
    Committing to one percent
        2012 olympics
        britain cycling team
            won 70% of medals
            before: none
        Dave Brailsford: coach
        aggregation of marginal gains
            small gains
            every aspect
    The Big Data Revolution
        3 important characteristics
            suspend disbelief of what is possible
            inherent knowledge of pattern recognition and insight to apply patterns
            commitment to one-percent improvement in every aspect

## Introduction

    storytelling
        collection of stories
    objective
        management
        breaking down the barrier between
            who manage data
            who manage people
        how other organizations did and outsmarted
            first part
                innovation using data
            second part
                key patterns
            third part
                methodology
    outline
        part 1
            ch 1: farms
            ch 2: doctors
                how many decisions are based on opinions instead of facts
            ch 3: insurance
            ch 4: retail and fashion
            ch 5: customer relationships
            ch 6: intelligen machines
                wind turbine
                internet of things
            ch 7: government and society
                social media: public opinions, perceptions, public policy
            ch 8: corporate sustainability
            ch 9: weather and energy
        part 2
            ch 10: pattern recognition
            ch 11: why patterns have emerged
            ch 12: patterns in big data
        part 3
            ch 13: data opportunity
            ch 14: porsche
            ch 15: puma
            ch 16: methodology
            ch 17: architecture
            ch 18: business view
            ch 19: logical view
            ch 20: future

## CHAPTER 1: TRANSFORMING FARMS WITH DATA

    california 2013
        strawberry quality
            balance
                quality
                consistency of quality
                waste
        brief history of farming
            1700s: subsistence
            1800s: for profit
            1900s: power
            1950s: machine
        data era
            current era
            data
            intagibles
                knowledge
                insight
                decision making
        stats
            2.2 M farms in US
            110 K $ on pest control, fertilizer
        potato farming
            the crop is underground
            yield prediction
                key variables
                    groundcover
                        percentage of ground covered by greenn leaf
                        measuring
                            capturing imagery
            four issues
                time: data at regular intervals
                geography: large scale
                man power: decision makers remote 
                irrigation: primary factor in maturation
            CanopyCheck app
            data and agronomy benefit: 30-50 % yield productivity
        precision farming
            key components
                yield monitoring:
                yield mapping
                variable-rate fertilizer
                weed mapping
                variable spraying
                topography and boundaries
                salinity mapping
                guidance systems
                records and analyses
        capturing farm data
            data landscape for farming:
                sensors
                    devices in machinery
                        water
                        pesticides
                gis
                gps
    Deere & Company versus Monsanto
        deere
            tractor company
        monsanto
            biotechnology for farming
        integrated farming systems
            field-by-field recommendations 
                to increase yeald
                to optimize inputs
                to enhance sustainability
            6 steps
                data backbone: testing
                variable-rate fertility: adjusting prescriptions
                precision seeding: optimal spacing
                fertility and disease management
                yield monitor
                breeding: to increase genetic gain
            products
                FieldScripts
                    seeding prescriptions for farms
    Data Prevails
        The Climate Corporation
            2013: 930 M $ sold
            founded in 2006
            assumption:
                unrealized opportunity of 50 bushels of crop in each field
        Growsafe Systems
            studying cattle since 1990
            sensors in water troughs and feedlots
                track movements of cattle
                    consumption, weight, movement, behaviors
                goal: look for outliers to prevent a disease
    Farm of the Future
        in 2020 all farmers will have:
            digital machines
                acting as sensors
                many drones
                device management
            IT back office
            asset optimization
                useful life machines, optimizing location, managing tasks
            preventative maintenance
            predictable productivity
            risk management
                weather will be a simple variable
                major risk: catastrophic outliers
            real time decision making
                streaming data
            production variability
                
## CHAPTER 2: WHY DOCTORS WILL HAVE MATH DEGREES

    United states, 2014
        a doctor visit
            measurements 
                at one particular instant in time
                may be subject to error
                doesn't capture temporal variations
            discussion of symptoms
                judgment and experience to asses the situation
    The History of Medical Education
        scientific method: 19th century
            question
            hypothesis
            prediction
            testing
            analysis
        rise of specialists
            specialization types
                surgical
                internal
                age
                diagnostics or therapeutic
                organ based or technique based
            side effects
                less efficient
                    visit multiple specialists
                create biases
                paid more
    We have a problem
        Ben Goldacre
            book: Bad Science
            2012 Ted talk
                widespread selection bias in academic publishing
                    publication bias
                        nostradamus
                            errors are ignored
                        medical trials
                            ignores failed tests
                    half of all trials are buried
                        positive findings are twice as likely to be published
        Vinod Khosla
            doctors are human
                cognitive limitations of doctors
                    decide on a patient diagnosis in first 30 seconds
                        on gut reaction
                opinions dominate medicine
                disagreement is common
    Data Era
        shift from intuition to data
        collecting data
            Peter Diamandis
                book: Abundance
                    data and data analysis will be the ceo of your health
        Cellscope
            founded 2010
            product: smartphone-enable otoscope
            diagnose ear infections
                parent collects data in the home
        Telemedicine
            ability to provide healtcare remotely
            TrueColours
                online self-management system
                    to monitor symptoms and experiences
                answering questionnaires
                doctors
                    much better informed about patients
                    estimating effects of changes in treatments
                patients
                    regularity of prompt text arrives
                    mood data
                        to characterize nature of mood disorder
                        classifying patients based on evolution of mood ratings
        Innovating with data
            Grok Technologies
                Nasa technology transfer program
                regenerating bone and muscle
                    BioReplicates
                        create models of human tissue
                    Scionic
                        minimizing pain and inflammation
            Quanttus: cardiovascular wearable sensors
                cardiovascular disease
                    develops slowly and methodically
                    strikes suddenly
                critical: identifying it
                wearable sensors measure vital signs
                    respiration, heart rate, blood pressure
            Healthtap: nlp in medicine
                social aspect to medicine
                patient
                    ask a question
                    get an answer
                processing datasets using nlp to find answers
                process
                    question routed to the physician that can best answer
                    previous questions analyzed
        Implications of a data-driven medical world
            biotech
            pharma
            payors
            providers
            patients
    the future of medical school
        prepare with data skills

## chapter 3: revolutionizing insurance: why actuaries will become data scientists

    middle of somewhere, 2012
    short history
    actuarial science in insurance
        actuaries
            assesing risk and uncertainty
        Chris Lewin
            "An Overview of Actuarial History"
            pensions
                payment after retirement
            compound interest
            probability
            mortality data
    modern day insurance
    data era
        dynamic risk management
            automobile insurance
                usage based insurance
                    pay as you drive
                    pay how you drive
                ex: car insurance for a 22 y female
                    actuarial insurance
                        collect all data
                        demographic data
                        probability, mortality, compount interest
                        offer a policy
                    dynamic risk management
                        sensor in car
                        if drives well, next premium is lower
                        means for encouraging behavior change
        catastrophe risk
            Nicholas Stern
                extreme weather cost: 1% of gdp
                    underestimated
            modeling risk
                hurricane 1992
                    15.5 B loss
                AIR Worldwide
                    correctly estimated in real time
        open access modeling
            implicit effects of disasters
                disrupting the manufacturing supply chain
            open source catastrophe modeling
                Oasis Loss Modeling Framework
        Opportunities
            disasters in 2013: 192 B $
            data source
                corwdsourcing data collection
                satellite imagery

### Meta

This post is published in: http://datascience.mertnuhoglu.com/post/notes_from_book_big_data_revolution/

The source for raw content is in: https://github.com/mertnuhoglu/my-readings/blob/master/books/data_science/book_big_data_revolution.md

