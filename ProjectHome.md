## Objective ##
This is a platform writing in Python that can use variant data-mining algorithms to get results from a source (like matrix, text documents). Algorithms can using xml configuration to make them run one-by-one. <p>
E.g. at first, we may run PCA(principle components analysis) for feature selection, then we may run random forest for classification.<br>
Now, algorithms are mainly design for tasks can be done in a single computer, good scalability of the architecture allows you in a very short period of time to complete the algorithm you want, and use it in your project (believe me, it's faster, better, and easier than Weka). <p>
The another important feature is this platfrom can support text classification or clustering operation very good.<br>
<br>
<br>
<h2>Get start</h2>
<b>Just write code like this, you will get amazing result (a naive-bayes training and testing),</b>
<pre><code>    # load configuratuon from xml file
    config = Configuration.FromFile("conf/test.xml")
    GlobalInfo.Init(config, "__global__")

    # init module that can create matrix from text file
    txt2mat = Text2Matrix(config, "__matrix__")

    # create matrix for training (with tag) from a text file "train.txt"
    [trainx, trainy] = txt2mat.CreateTrainMatrix("data/train.txt")

    # create a chisquare filter from config file
    chiFilter = ChiSquareFilter(config, "__filter__")

    # get filter model from training matrix
    chiFilter.TrainFilter(trainx, trainy)

    # filter training matrix
    [trainx, trainy] = chiFilter.MatrixFilter(trainx, trainy)

    # train naive bayes model
    nbModel = NaiveBayes(config, "naive_bayes")
    nbModel.Train(trainx, trainy)

    # create matrix for test
    [testx, testy] = txt2mat.CreatePredictMatrix("data/test.txt")

    # using chisquare filter do filtering
    [testx, testy] = chiFilter.MatrixFilter(testx, testy)

    # test matrix and get result (save in resultY) and precision
    [resultY, precision] = nbModel.Test(testx, testy)

    print precision
</code></pre>

And you need define some paramters in a configuration file (It will be loaded by Configuration.FromFile(...xml)).<br>
<br>
<pre><code>    &lt;config&gt;
        &lt;__segmenter__&gt;
            &lt;main_dict&gt;dict/dict.main&lt;/main_dict&gt;
        &lt;/__segmenter__&gt;

        &lt;__matrix__&gt;
        &lt;/__matrix__&gt;

        &lt;__global__&gt;
            &lt;term_to_id&gt;mining/term_to_id&lt;/term_to_id&gt;
            &lt;id_to_term&gt;mining/id_to_term&lt;/id_to_term&gt;
            &lt;id_to_doc_count&gt;mining/id_to_doc_count&lt;/id_to_doc_count&gt;
            &lt;class_to_doc_count&gt;mining/class_to_doc_count&lt;/class_to_doc_count&gt;
            &lt;id_to_idf&gt;mining/id_to_idf&lt;/id_to_idf&gt;
            &lt;newid_to_id&gt;mining/newid_to_id&lt;/newid_to_id&gt;
        &lt;/__global__&gt;

        &lt;__filter__&gt;
            &lt;rate&gt;0.2&lt;/rate&gt;
            &lt;method&gt;max&lt;/method&gt;
            &lt;log_path&gt;mining/filter.log&lt;/log_path&gt;
            &lt;model_path&gt;mining/filter.model&lt;/model_path&gt;
        &lt;/__filter__&gt;

        &lt;naive_bayes&gt;
            &lt;model_path&gt;mining/naive_bayes.model&lt;/model_path&gt;
            &lt;log_path&gt;mining/naive_bayes.log&lt;/log_path&gt;
        &lt;/naive_bayes&gt;

        &lt;twc_naive_bayes&gt;
            &lt;model_path&gt;mining/naive_bayes.model&lt;/model_path&gt;
            &lt;log_path&gt;mining/naive_bayes.log&lt;/log_path&gt;
        &lt;/twc_naive_bayes&gt;
        
        &lt;smo_csvc&gt;
          &lt;model_path&gt;mining/smo_csvc.model&lt;/model_path&gt;
          &lt;log_path&gt;mining/smo_csvc.log&lt;/log_path&gt;
          &lt;c&gt;100&lt;/c&gt;
          &lt;eps&gt;0.001&lt;/eps&gt;
          &lt;tolerance&gt;0.000000000001&lt;/tolerance&gt;
          &lt;times&gt;50&lt;/times&gt;
          &lt;kernel&gt;
            &lt;name&gt;RBF&lt;/name&gt;
            &lt;parameters&gt;10&lt;/parameters&gt;
          &lt;/kernel&gt;
          &lt;cachesize&gt;300&lt;/cachesize&gt;
        &lt;/smo_csvc&gt;
    &lt;/config&gt;
</code></pre>

<h2>Features</h2>
<b>Clustering algorithm</b>
<ul><li>KMeans</li></ul>

<b>Classification algorithm</b>
<ul><li>Random forest<br>
</li><li>Naive Bayes<br>
</li><li>TWC Naive Bayes<br>
</li><li>SVM</li></ul>

<b>Feature selector</b>
<ul><li>Chisquare<br>
</li><li>PCA</li></ul>

<b>Mathematic</b>
<ul><li>Basic operations (like bagging, transpose, etc.)</li></ul>

<b>Data source support</b>
<ul><li>Matrix (with csv format)<br>
</li><li>Raw text (now only support Chinese, English to be added)