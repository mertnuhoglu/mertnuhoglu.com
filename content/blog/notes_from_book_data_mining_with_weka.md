---
title: "Notes From Book: Data Mining with Weka Mooc - Ian H. Witten"
date: 2017-11-23T15:56:51+03:00
description: ""
tags:
categories: ["datascience", "book_notes"]
type: post
draft: false

---

My study notes from the video training.

<!--more-->

<!-- toc -->

## 03-Data Mining with Weka (1.2 - Exploring the Explorer)-nHm8otvMVTs.mp4

    example data set
        instances
        attributes
    weka
        explorer > open file > 
            inside weka > data > weather.nominal.arff
        attributes
            classes
            select > labels
            attribute values
                counts
        class diagram
            usually the last variable is class (label)
        > edit
            data in table view
            > change data in any cell

## 04-Data Mining with Weka (1.3 - Exploring datasets)-BO6XJSaFYzk.mp4

    predict class of weather
    classification problem
        called: supervised learning
    classified example
        instance
            attribute 1, ... attribute n, class
        instance: fixed set of features
            discrete: nominal
                classification problem
            continuous: numeric
                regression problem
    open > weather.numeric.arff
    open > glass.arff
        check glass.arff
            # comments
            # attribute information
            @relation Glass
            @attribute 'RI' real
            @attribute 'Type { 'build wind float', ... }
            @data
            10,20,10,'build wind float'
    sanity checking attributes

## 1.4 Building a classifier

    weka > classifiy > choose > trees > J48
    output
        confusion matrix
            classified as a, while labeled as a
            a  b  c  d  e  f  g   <-- classified as
            50 15  3  0  0  1  1 |  a = build wind float
            16 47  6  0  2  3  2 |  b = build wind non-float
            5  5  6  0  0  1  0 |  c = vehic wind float
    build a configuration panel
        choose > J48 > click > .unpruned: T
    output
        compare two runs
            Summary > correctly classified instances 
                = accuracy
    3rd run
        choose > j48 > click > min number of instances: 15
            make larger leaves
    visualize tree
        output > run > right > visualize tree
        resize window > right > fit to screen
    configuration panel
        > more
            documentation of the parameters
        
## 1.5 Using a filter

    preprocessing (filtering) data before classifying
    open weather.nominal.arff
        choose
            filters > unsupervised > attribute > remove
            > configure > .attribute: 3
            > apply
            3. attribute is removed
        remove only when humidiy = high
            filters > unsupervised > instance > removewithvalues
            > configure > .indic: 3, value: 1
            > apply

## 1.6 Visualizing your data

    open iris.arff
    visualize
        matrix
            attribute x attribute
        > click one diagram 
            > click a data point
                instance: 86
                sepallength: 6
                ...
            change x, y axes
                sidebar
                    click: x axis
                    right: y axis
            jitter
                biraz sallar, aynı yerdeki noktaları
            selection
                > rectangle
                    > draw a rectangle selection > submit
        classify > ...
            run > right > visualize errors
                predicted class
                class
                > click errors
                    same as confusion matrix

## 2.1 Be a classifier

    open: segment.challenge.arff
        image analysis dataset
        attributes
            centroid
            saturation
            hue
            class: texture
                brick
                sky
        classify > tree > userclassifier
            > test options: supplied test set
                > .file: segment-test.arff
            > start
                > data visualizer 
                        x: region-centroid-raw
                        y: intensity-mean
                    select rectangle region > submit
                        > tree visualizer
                            refined tree
            logic
                user selects a cluster
                weka makes a tree branch for it
            use x-y axes splits
                to separate clusters better
            > tree visualizer
                right > accept the tree 
            < confusion matrix

## 2.2 Training and Testing

    logic
        training data > ml algorithm > classifier
        test data > classifier > evaluation results
        different
            test data
            training data
            independent sampling from population
    lesson
        open: segment-challenge.arff
        choose: j48
        supplied test set: segment-test.arff
        run: 96% accuracy
        eval on training: 99% accuracy
            too much: misleading results
        eval on percentage split: 95%

## 2.3 Repeated training and testing

    lesson
        segment-challenge.arff
        j48
        set percentage split to
            90% -> 96% acc
            repeat with seed
                2,3,4
    how
        more options
            random seed: 2%
    given lots of accuracies
        mean of accuracy?
        variance?

## 2.4 Baseline accuracy

    lesson
        open: diabetes.arff
        test option: percentage split
        try classifiers:
            trees > j48
            bayes > naivebayes
            lazy > lbk
            rules > part
    steps
        diabetes.arff
            try classifiers:
                trees > j48                 76%
                bayes > naivebayes  77%
                lazy > lbk                  73%
                rules > part                74%
            is this good?
                class: 
                    500 negative
                    268 positive
                always guess "negative":
                    500/768 = 65%
                    rules > zeror classifier
        supermarket.arff
            zeror 63%
            j48     62%
                worse than zeror
            naivebayes  62%
            ibk     38%
            why?
                attributes are not informative
                you need to understand what's going on
                always try simple classifiers (baseline first ZeroR)

## 2.5 Cross-validation

    logic
        can we improve upon repeated holdout?
            that is reduce variance
        cross-validation
            way of reducing variance
        stratified cross-validation
            reduces even further
    repeated holdout
        hold out 10% for testing
        repeat 10 times
        use some other 10% for testing everytime
        10-fold cross-validation
            divide dataset into 10 parts(folds)
        stratified cross-validation
            each fold has right proportion of each class values
        after cross-validation
            weka runs 11th time 
                on 100% data
        rules of thumb
            use percentage split if lots of data

## 2.6 Cross-validation results

    is cross-validation better than repeated holdout?
        diabetes dataset
        baseline: 65%
        trees > j48
        10-fold cross-validation 73.8%
    how
        test options: cross-validation. folds: 10
        more options > random seed: change
    results
        holdout
            mean: 74.8
            std.dev: 4.6
        cv
            mean: 74.5
            sd: 0.9
    conclusion
        it reduces variance of estimate
        standard: 10-fold cv

## 3.1 Simplicity first

    logic
        simple algorithms often work very well
        many kinds
            one attribute does all the work
            all attributes contribute equally
            decision tree
            calculate distance
            result depends on linear combination of attributes
    OneR: one attribute does all the work
        1 level decision tree
            or a set of rules
        basic
            one branch for each value
            each branch assigns most frequent class
    ex
        attribute           rules               errors
        outlook             sunny > no  2/5
                                    over > yes  0/4
                                    rainy > yes 2/5
        temp                    hot > no
                                    mild > yes
                                    cool > yes
    steps
        weather.symbolic.arff
    how can it work well?
        some datasets are simple
        some are so noisy that nothing can be learned from them

## 3.2 Overfitting

    logic
        works well on training data
            not on independent test data
    lesson
    steps
        weather-numeric
            OneR
                configure > min bucket size: 1
                    overfit
        interesting
            using training set
                high accuracy when overfitting
            using cv
                low accuracy

## 3.3 Using probabilities

    logic
        oner: one attribute does all the work
        opposite strategy: use all attributes
            naive bayes method
        assumptions
            attributes are
                equally important
                independent
                    this is never correct
        bayes theorem
            p(h | e)
                h: hypothesis (event)
                e: instance (evidence)
                = p( e | h ) p(h) / p(e)
            in general
                p(h|e) = p(e1|h) p(e2|h) ... p(h) / p(e)
                p(h) 
                    a priori probability of h
                    probability of event before evidence is seen
                p(h|e)
                    a posteriori probability of h
                    probability of event after evidence is seen
            naive assumption
                evidence splits into parts that are independent

## 3.4 Decision trees       

    which attribute to select
        we don't want mixtures in a branch
        how to quantify this?
        aim: get smallest tree
        heuristic
            information theory based
            shannon
            information gain
                entropy before split - entropy after split
        j48
            best 10 years ago
            very easy to understand output

## 3.5 Pruning decision trees

    highly branching attributes
        extreme case: id code
            split each instance as a new branch
            but doesn't generalize to new instances
        how to prune?
            don't continue splitting if nodes get very small
                j48: 
                    minNumObj: default 2
                    confidenceFactor
                    subtreeRaising

## 3.6 Nearest neighbor

    logic
        rote learning: simplest form of learning
            called also: 
                instance based learning
                nearest neihbor learning
                lazy learning
                    do nothing until you make predictions
        to classify a new instance
            search training set for one that's most like it
        ex
            take a point
            what is the closest point to it
            what is its class?
        what is most like?
            need a similarity function
                regular distance (euclidean)
                manhattan (city-block) distance
                    sum of absolute differences
                nominal attributes
                    1 if different
                    0 if same
        noisy instances?
            if we have noisy data, then by accident we can classify to unknown point
            use k-nearest neighbor
            weka: ibk: instance based learning k
        assumes
            all attributes equaly important
            remedy: attribute selection or weight
        noisy instances
            k nearest neighbors
            weight instances 
            identify reliable prototypes
        n -> infinity
            
## 4.1 Classification boundaries

    classification boundaries
    steps
        open: iris-2d
        weka > visualization > boundary visualizer
        open: iris-2d again
        classifier > choose > rules > OneR
    summary
        classifiers create boundaries in instance space
        classifiers have different biases
        visualization restricted to
            2d plots
            numeric attributes

## 4.2 Linear regression

    regression
        predict numeric value
    weka
        functions > linear regression
    non linear regression
        model tree
            each leaf has a linear regression model
            combination of tree and linear regression
    weka
        trees > m5p

## 4.3 Classification by regression

    how to use regresion technique for classification
        two class problem
            call classes: 0 and 1
            set a threshold for predicting class 0 or 1
        multi class problem
            training:
                perform a regression for each class
            prediction
                choose the class with largest output
    steps
        open: diabetes
        filter > unsupervised > attributes > NominalToBinary
            attribute indices: 9 (class)
        class: none
        classify > linearregression
            > more options: output predictions
                inst#,    actual, predicted, error
                        1      0          0.325      0.325
                        2      0          0.308      0.308      
    extend linear regression to classification
        add classification attribute
            filter > AddClassification
                configure
                    classifier: LinearRegression    
                    outputClassification: True
        convert class to nominal again
            filter > NumericToNominal
                configure
                    indices: 9
            remove all variables except class and classification
        predict/classify
            choose > LinearRegression
            target: class

## 4.4 Logistic regression

    better prediction by probabilities
        other methods
            naive bayes produces them
                columns: actual, predicted, error, prob distribution
                options: output prediction
            ZeroR
                adds 1 to each class
            J48
                negative, positive probability
    logistic regression
        linear
            calculate a linear function and then a threshold
        logistic
            estimate class probabilities directly
            S like function
            maximize log-likelihood
                not minimize SSE

## 4.5 Support vector machines

    logic
        logistic regression 
            produces linear boundaries
        how to produce linear boundaries with widest distance
            perpendicular bisector from
                support vectors
                    they are either 2 or 3 or 4 points
                    interior points are not important
        not all classes are linearly separable
        very resilient to overfitting
            because depends on very few points
    steps
        functions>smo
            two classes only
        functions>LibSVM
            external library
                
## 4.6 Ensemble learning

    logic
        as if experts vote
        output: hard to analyze
        methods
            bagging
            randomization
            boosting
            stacking
        bagging
            logic
                several training sets of same size
                    sampling with replacement
                build model for each one
                use ml
                combine predictions by voting
            very good for unstable learning schemes
                ex: decision trees
            weka
                meta > Bagging
        randomization
            logic
                random forests
                    uses decision tree
                    randomizes algorithm not training data
                        picks attributes not best but from k best option randomly
            weka
                trees > RandomForest
        boosting
            logic
                iterative
                    new models influenced by old ones
                    extra weight for instances that are misclassified
                    intuitive: members should complement each other
        weka
                meta > Adaboosting
        stacking
            logic
                base learners: level-0 models
                meta learner: level 1 model
                predictions of base learners are input to meta learner
            weka
                meta > Stacking


# More Data Mining

## 1.2 Exploring the Experimenter

    use for
        mean and std of an algorithm on dataset
        is one classifier better?
        is one parameter better?
        computation can be distributed
    steps
        weka > experimenter
        new
            datasets > add new > .segment.arff
            algorithms > add new > .j48
        run > start
        analyse 
            experiment
            perform test
                show std: T
    what about individual results of each run
        setup > .results destination: csv
            experiment type: percentage split
                train percentage: 90
        run > start
        open csv file
            repeated experiment 10 times
            "percent_correct"

## 1.3 Comparing classifiers

    logic
        is j48 better than zeror on iris data?
    steps
        experimenter > new
        data set: iris
        algorithm: add new
            j48
            zeror
            oner
        run > start
        analyze > experiment
            perform test
        results
            Dataset                   (1) rules.Ze | (2) rules (3) trees
            ------------------------------------------------------------
            iris                     (100)   33.33 |   92.53 v   94.73 v
            ------------------------------------------------------------
                                                                         (v/ /*) |   (1/0/0)   (1/0/0)
            meanings
                * significantly worse
                v significantly better
        add new multiple data sets
        analyze
            configure test > test base
                change base
                results: y1 is better than x
            row, column
                change datasets and algorithms
                algorithm is better in x dataset

## 1.4 Knowledge Flow interface

    knowledge flow interface
        alternative to explorer
    steps
        weka > knowledge flow
        datasources > ArffLoader
            right > dataset
        evaluation > ClassAssigner
            right > dataset
        evaluation > CrossValidationFoldMaker
            right > trainingSet, testSet
        classifiers > J48
            right > batchClassifier
        evaluation > ClassifierPerformanceEvaluator
            right > text
        Visualization > TextViewer
        running
            ArffLoader > Start Loading
            TextViewer > show
    working with stream data
        ArffLoader => ClassAssigner
            right: instance
        ClassAssigner => NaiveBayesUpdateable 
            instance
        NaiveBayesUpdateable => Incremental ClassifierEvaluator 
            incremental
        => StripChart
            chart
        StripChart > view
        ArffLoader > start loading
        2015-08-09_12-51-05.png

## 1.5 Command Line interface

    weka > Simple CLI
        java weka.classifiers 13:23:07rees.J48
        explorer > classify > classifier
            default parameters
            => right > copy 
                2015-08-09_13-25-29.png
        java weka.classifiers.trees.J48 -C 0.25 -M 2 -t /Users/mertnuhoglu/data/iris.arff
            J48 options
                -t training_file
                -T test_file
        classes and packages
            weka.classifiers.trees.J48
                class: J48
            javadoc: weka/doc/index.html
                options are documented here
    open database
        weka > explorer > open db
        database converter
            weka.core.converters.DatabaseConverter
        
## 1.6 Working with big data

    explorer
        ~ 1 M instances
        status > right > memory
    generate data
        explorer > generate data
            choose: LED24
            params: 
                num: 100
        too much data -> crash
            explorer: loads all data
            updateable classifiers
                incremental classification models
    how much data can weka handle?
        unlimited if incremental
    incremental cli
        generate data
            java weka.datagenerators.classifiers.classification.LED24 -n 1000000 -o /Users/mertnuhoglu/data/train.arff
                puts generated data into test.arff
        classify
            java weka.classifiers.bayes.NaiveBayesUpdateable -t /Users/mertnuhoglu/data/train.arff -T /Users/mertnuhoglu/data/test.arff
        classifier implementations with "Updateable"
            find from javadoc

## 2.1 Discretizing numeric attributes

    discretizing
        transform numeric to nominal
            equal width binning
            equal frequency binning
                or histogram equalization
            how many bins?
            exploit ordering information?
    equal width binning
        open: ionespeher.arff
        filter > discretize
            params: 
                numBins: 40
            2015-08-09_15-27-45.png
        classify:
            worse accuracy
        discretize again
            undo first
            filter> discretize > numBins: 2
            better accuracy
        discretize again
            undo
            filter > discretize >
                equalFrequency: T
            2015-08-09_15-31-06.png
        experiment 
            different numBins
            equal frequency binning
    ordering information
        how to use it?
        in numeric, there is ordering
            in nominal, there is no ordering
            can't use in decision trees like
                y > x
                instead
                    y = a, y = b
                    not efficient
        solution
            instead of k values
            make k-1 binary attributes
            comparison
                x <= v
                equivalent to
                z3?
                    given v is in z3
        weka
            filter > discretize > params
                makeBinary: T

## 2.2 Supervised discretization and the FilteredClassifier

    supervised discretization
        take class value into account
        2015-08-09_15-52-31.png
        move boundaries towards labeled class boundaries
    use entropy heuristic
        ex: J48
        entropy before: 0.934 bits
        choose split point with smallest entropy
        repeat recursively until stop
        weka:
            filter > supervised > discretize
            problem:
                this uses cross-validation
                cv uses test data
                which is not good
        solution:
            classify > meta > FilteredClassifier
                params
                    classifier: J48
                    filter: supervised > Discretize

## 2.3 Discretization in J48

## 2.4 Document classification

    ex
        documents
            price of crude oil
            this meat is oily
        class (type)
            yes, no
        weka > filter > StringToWordVector
            2015-08-09_16-53-18.png
            all words become an attribute
            values:
                1: it appears
                0: it doesn't appear in doc
        weka > classify > j48
            set class attribute
            opt: use training set
            result
                tree:
                    if no "crude" => no
                    if "crude" => yes
    how to classify a test doc
        weka: 
            error: test data and train data are not compatible
                test data is string text
                convert by StringToWordVector
                but still attributes are different
            solution: use FilteredClassifier
        weka:
            classify: FilteredClassifier
                params
                    classifier: J48
                    filter: unsupervised > StringToWordVector
                options
                    output predictions: T
    ex2:
        open: ReutersCorn-train.arff
        filter: StringToWordVector
            out: 2234 attributes
            undo this
        classify: FilteredClassifier
            supplied test set: Reuters-test.arff
        result:
            decision tree
                if corn
                    if planted
                        then classify as "corn"
                    else if 1986
                        if maize
                            if the
                                then "corn"
            eval
                97% accuracy
                    but
                        62% on 24 corn related docs
                        99% on remaining 580 docs
                    thus
                        overall classification accuracy is not the right thing to optimize

## 2.5 Evaluating 2-class classification

    confusion matrix
        x axis
            real label
        y axis
            classified as
        ex
            a b   <-- classified as
            7 2 | a = yes
            4 1 | b = no
        terms
            true positive       false negative
            false positive  true negative
        accuracy
            on a
                true positive / class a
            on b
                true negative / class b
        tradeoff
            accuracy on a vs. b
            ROC curve
                2015-08-09_18-32-56.png
                goal: top left corner
                you can put the threshold at other points => different accuracies
            AUC
                area under the curve
                as large as better
        weka > result > right > visualize threshold curve
            2015-08-09_18-37-26.png

## 2.6 Multinomial Naive Bayes

    naive bayes
        evidence splits into independent parts
            p(e|h) = p(e1|h)p(e2|h)...p(en|h)
        document classification
            e_i: appearance of word i
        problems
            non-appearance of word counts just as strong
            does not account repetitions of word
            treats all words the same (common, unusual)
    multinomial naive bayes
        solves the above issues
    weka
        FilteredClassifier
            NaiveBayesMultinomial
            filter: StringToVector
                outputWordCounts: T
                lowerCaseTokens: T
                useStopList: T
                    disregard common words
        
## 3.1 Decision trees and rules

    decision trees and rules
        every path is a rule
            if outlook = sunny and humidiy = high then no
        rules and trees have equivalent expression power
        but rules are simpler to read
        rules depend on sequence implicitlyr    

## 3.2 Generating decision rules

    PART: rules from partial decision trees
        separate and conquer
            make a rule
            remove instances it covers
            continue
    Ripper: weka: jrip. incremental reduced-error pruning
        PRISM: exact rules
        ripper: splits training set into two
            for each class C
                grow
                    use prism to find best rule for C
                prune
    ex
        jrip
            rules
                (plas >= 132) and (mass >= 30) => class=tested_positive (182.0/48.0)
                (age >= 29) and (insu >= 125) and (preg <= 3) => class=tested_positive (19.0/4.0)
                (age >= 31) and (pedi >= 0.529) and (preg >= 8) and (mass >= 25.9) => class=tested_positive (22.0/5.0)
                => class=tested_negative (545.0/102.0)
                Number of Rules : 4

## 3.3 Assoiation rules

    association rules
        no class attribute
        rules predict combination of attributes
        ex
            humidity=normal & windy = falsa ==> play = yes
        support: 
            number of instances that satisfy rule
        confidence
            proportion that satisfy lhs for which rhs also holds
        logic
            specify minimum confidence
            seek rules with greatest support
        terms
            itemset
                set of attribute-value pairs
                    humidity = normal & windy = false & play = yes
                        support = 4
                        potential rules for this itemset
                            if humidity = normal & windy = false ==> play = yes
                            if humidity = normal & play = yes ==> windy = false
                    2015-08-10_10-02-22.png

## 3.4 Learning association rules

    steps
        open: weather.nominal.arff
        associate > choose > Apriori
        output
            10 rules
            conf: (1)
                confidence %100
        params
            minimum support: 0.15
                0.15 * 14 = 2 instances
            minimum (metric) confidence: 0.9
            outputItemSets: T

## 3.5 Representing clusters

    representing clusters
        no class attribute
        divide instances into natural groups/clusters
        ex
            imagine deleting class attribute
            could you recover classes by clustering data
        cluster types
            disjoint sets
            overlapping sets
            2015-08-10_10-22-35.png
            probabilistic clusters
            hierarchical clusters
            2015-08-10_10-25-14.png
        KMeans
            iterative distance based clustering (disjoint sets)
            algorithm
                specify k, number of clusters
                choose k points at random as cluster centers
                assign all instances to their closes cluster center
                calculate centroid (mean) of instance in each cluster
                these centroids are new cluster centers
                continue
            minimizes total squared distance from instances to cluster centers
                local minimum
                different results with different random seeds
            weka: SimpleKMeans
                params
                    numClusters
                    distanceFunction
                    seed
                open: weather.numeric.arff
        XMeans
            extended version of KMeans
            logic
                selects number of clusters itself
                cannot handle nominal attributes
        EM clustering
            probabilistic
            Expectation Maximization
            params
                numClusters
            prior probabilities
        Cobweb clustering (hierarchical)
        hard to evaluate clustering

## 3.6 Evaluating clusters

    visualizing clusters
        open: iris
        cluster > SimpleKMeans
            ignore: class attribute
        result > visualize cluster assignments
    which instances does a cluster contain?
        filter > unsupervised > AddCluster 
            SimpleKMeans
                numCluster: 3
        2015-08-10_11-40-55.png
    classes-to-clusters evaluation
        weka
            filter > undo
            cluster > classes to clusters evaluation
            output
                0  1  2  <-- assigned to cluster
                0 50  0 | Iris-setosa
                47  0  3 | Iris-versicolor
                14  0 36 | Iris-virginica
    ClassificationViaClustering meta classifier
        logic
            ignore class
            assign classes after clusters


## 4.1 Attribute selection using the wrapper method

    logic
        fewer attributes, better classification
        use "select attributes" to find best attributes
    weka
        Select attributes > attribute evaluater > WrapperSubsetEval
            classifier: J48 
            folds: 10
            threshold: -1
        Search method: BestFirst
            direction: Backward
    searching
        backward and forward    
            2015-08-10_12-01-21.png
        exhaustive search: 2^9 = 512 subsets
        when to stop?
            searchTermination
            local optimum
        
## 4.2 Attribute Selected Classifier

    attribute selected classifier
        logic
            select attributes and apply a classifier to result
            cheating?
                yes, we use the entire training set
                    on attribute subset
            like FilteredClassifier
            weka: meta > AttributeSelectedClassifier
                train classifier
                evaluate on test data only

## 4.3 Scheme independent attribute selection

    logic
        wrapper method is slow
        weka: CfsSubsetEval
            attribute is good if attributes it contains are
                highly correlated with class 
                not strongly correlated with one another
        
## 4.5 Counting the cost

    what is success
        classification rate
        but in real life
            different errors have different costs
            minimizing total errors is inappropriate
                with 2 class classification, 
                    ROC summarizes different tradeoffs
        ex
            credit-g.arff
                worse: 
                    if bad customer is classified as good
            confusion matrix
                a   b   <-- classified as
                588 112 |   a = good
                183 117 |   b = bad
            cost:
                183 x 5 + 112 x 1
            weka > options
                cost sensitive evaluation
                    2015-08-10_12-38-09.png
            out
                Total Cost 1027     
                Average Cost 1.027 
            baseline
                everything good
                    weka > zeroR
                    total cost 1500
                everything bad
                    total cost 700
        wake: cost-sensitive classification
            meta > CostSensitiveClassifier
                classifier: J48
                cost matrix

## 4.6 Cost sensitive classification vs. cost sensitive learning

    content
        making c calssifier cost sensitive
            opt
                cost sensitive classification
                cost sensitive learning
    cost sensitive classification
        ex: NaiveBayes
            threshold: 0.5
            recalculating probability threshold
                cost matrix: 0 1/5 0
                threshold: 5/6 = 0.833
        what about methods without probabilities
            ex: J48
                get mistake probabilities for each leaf
                2015-08-10_12-57-36.png
        weka 
            meta > CostSensitiveClassifier
                classifier: J48
                costMatrix
                minimizeExpectedCost: T
    cost sensitive learning
        logic
            instead of adjusting output of classifier
            some instances replicated
                add 4 copies of every bad instance
        weka    
            meta > CostSensitiveClassifier
                classifier: J48
                costMatrix
                minimizeExpectedCost: F

## 5.1 Simple neural networks

    perceptron: simplest form
        a hyperplane that separates points
            w0 + a1 w1 + ... + an wn = 0
        learning rule
            if a_i w > 0
                then it belongs to first class
                if this is true, good
                if not
                    (w0 + a0) a0 + ... + (wn + an) an
                        move the point towards other side of hyperplane

## 5.2 Multilayer Perceptrons

    multilayer perceptrons
        network of perceptrons
            input layer
            hidden layers
            output layers
    how many layers, how many nodes in each?
        input layer
            one per attribute
        output layer
            one per class
        hidden layers?
            zero
                standard perceptron
                if data is linearly separable
            one
                single convex region
            multiple
                arbitrary decision boundaries
        how many?
            heuristic
    what are weights
        minimize error using steepest descent
            gradient determined using backpropagation
    weka: 
        classifier: MultilayerPerceptron
            hiddenLayers: 5,10,20
                Gui: T
            learningRate,
            momentum
        creating custom network
            right: new node

## 5.3 Learning curves

    how much data do i need?
        if not large data set
            use 10-fold cross validation
    plotting a learning curve
        sampling
            with replacement 
                copy
            or no replacement
                move
            Sample training set but not test set
        weka
            meta > FilteredClassifier
                classifier: J48
                filter: unsupervised > Resample
                    noReplacement: T
                    sampleSizePercent: 65%
            2015-08-10_14-09-22.png

## 5.4 Meta-learners for performance optimization   

    logic
        like Wrapper: AttributeSelectedClassifier withWrapperSubsetEval
            selects an attribute subset based on how well a classifier performs
        CVParameterSelection
            select best value for a parameter
        GridSearch
            optimizes two params
        ThresholdSelector
            select probability threshold
            to optimize
                accuracy
                true positive rate
                precision
                ...
    weka
        meta > CVParameterSelection
            classifier: J48
            CVParameters
                C 0.1 0.9 9
                2015-08-10_14-25-14.png
        meta > GridSearch
            optimizes two params together
            XProperty: filter
            YProperty: classifier
        meta > ThresholdSelector
            optimize other things
                Precision: TP / (TP+FP)
                Recall = TP / (TP+ FN)
                    mnemonics
                        TP+FP
                            classified good
                        TP+FN
                            actually good

## 5.5 ARFF and XRFF

    arff format
        structure
            @relation
            @attribute
            @data
            data lines (?: missing)
            % comment lines
        sparse arff
            specify only non-default values
                {3 FALSE, 4 no}
                    3 column: non-default. F
                    4 column, non-default: no
        weighted instances
            sunny, 85, no, {0.5}
                weight this instance as 0.5 instance
        date attributes
        relational attributes (multi-instance learning)

## 5.6 Summary

    missing
        subjects
            time serieas analysis
            stream-oriented algorithms
                MOa system
            multi-instance learning
            one-class classification
            interfaces to other dm packages
            distributed weka with hadoop
            latent semantic analysis
        available as weka packages
        
### Meta

This post is published in: http://datascience.mertnuhoglu.com/post/notes_from_book_data_mining_with_weka/

The source for raw content is in: https://github.com/mertnuhoglu/my-readings/blob/master/books/data_science/book_data_mining_with_weka.md



