Hello World Benchmark
=====================
Como os benchmarks foram realizados?
----------------------------------
Criamos um benchmark "Hello World" procurando identificar a menor carga de cada um dos frameworks. Muitas pessoas não gostam desses tipos de benchmark, pois uma  aplicação no mundo real requer estruturas e recursos mais complexos. Entretanto esses testes identificam o menor tempo gasto por cada framework para executarem uma tarefa simples. Tal tarefa representa o menor requerimento para cada framework processar uma única requisição.

Para ser mais específico, o benchmark somente mede o tempo gasto por cada framework para startar, executar uma ação (action) e liberar os recursos no final da requisição. Qualquer aplicação PHP baseada na arquitetura MVC precisará  deste tempo. A despeito da simplicidade do benchmark, nós certificamos que o tempo necessário para uma atividade mais complexa será maior.

Uma controladora e uma view foram criadas para cada framework: a controladora “say” e uma action “hello”. A action somente envia dados para a view na qual mostra: "Hello!". Utilizando a ferramenta de benchmark “ab” nós enviamos 2000 requisições usando 10 conexões concorrentes para cada framework. 

Quais foram as medidas registradas? 
--------------------------------
Estas foram as medidas registradas para identificar o desempenho geral de cada framework: 

* Requisições por segundo
* Tempo em todas as requisições simultâneas. 
* Quantidade de arquivos PHP inclusos em uma única requisição (mensurado utilizando a função get_included_files_ ). 
* Utilização da memória por cada requisição (mensurado utilizando a função memory_get_usage_ ). 

Frameworks Participantes
-----------------------
* Yii_ (YII_DEBUG=false) (yii-1.1.13)
* Symfony_ (2.0.11)
* `Zend Framework`_ (1.11.11)
* Kohana_ (3.2.0)
* FuelPHP_ (1.2.1)
* CakePHP_ (2.1.3)
* Laravel_ 3.2.5
* CodeIgniter_ (2.1.0)
* Nette_ (2.0.4)

Resultados
-------
Yii (YII_DEBUG=false) Version yii-1.1.13
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. code-block:: php

    # ab -n 2000 -c 10 http://localhost/bench/helloworld/yii/index.php?r=say/hello
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/

    Benchmarking localhost (be patient)


    Server Software:        Apache/2.2.22
    Server Hostname:        localhost
    Server Port:            80

    Document Path:          /bench/helloworld/yii/index.php?r=say/hello
    Document Length:        61 bytes

    Concurrency Level:      10
    Time taken for tests:   2.081 seconds
    Complete requests:      2000
    Failed requests:        0
    Write errors:           0
    Total transferred:      508000 bytes
    HTML transferred:       122000 bytes
    Requests per second:    961.28 [#/sec] (mean)
    Time per request:       10.403 [ms] (mean)
    Time per request:       1.040 [ms] (mean, across all concurrent requests)
    Transfer rate:          238.44 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0   10   4.3      9      42
    Processing:     0    0   1.0      0      24
    Waiting:        0    0   0.8      0      17
    Total:          3   10   4.3      9      42

    Percentage of the requests served within a certain time (ms)
      50%      9
      66%     11
      75%     13
      80%     14
      90%     15
      95%     17
      98%     21
      99%     26
     100%     42 (longest request)

Symfony Version 2.1.6
^^^^^^^^^^^^^^^^^^^^^^
.. code-block:: php

    # ab -n 2000 -c 10 http://localhost/bench/Symfony/web/app.php/say/hello/
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/

    Benchmarking localhost (be patient)


    Server Software:        Apache/2.2.22
    Server Hostname:        localhost
    Server Port:            80

    Document Path:          /bench/Symfony/web/app.php/say/hello/
    Document Length:        16 bytes

    Concurrency Level:      5
    Time taken for tests:   1.848 seconds
    Complete requests:      1000
    Failed requests:        0
    Write errors:           0
    Total transferred:      249000 bytes
    HTML transferred:       16000 bytes
    Requests per second:    541.01 [#/sec] (mean)
    Time per request:       9.242 [ms] (mean)
    Time per request:       1.848 [ms] (mean, across all concurrent requests)
    Transfer rate:          131.55 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0    9   4.8      8      61
    Processing:     0    0   0.6      0      15
    Waiting:        0    0   0.6      0      15
    Total:          4    9   4.8      8      61

    Percentage of the requests served within a certain time (ms)
      50%      8
      66%      9
      75%     11
      80%     12
      90%     15
      95%     18
      98%     22
      99%     30
     100%     61 (longest request)

CodeIgniter 2.1.0
^^^^^^^^^^^^^^^^^
.. code-block:: php

    # ab -n 2000 -c 10 http://localhost/bench/codeigniter/index.php/say/hello
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/

    Benchmarking localhost (be patient)


    Server Software:        Apache/2.2.22
    Server Hostname:        localhost
    Server Port:            80

    Document Path:          /bench/helloworld/codeigniter/index.php/say/hello
    Document Length:        16 bytes

    Concurrency Level:      10
    Time taken for tests:   1.888 seconds
    Complete requests:      2000
    Failed requests:        0
    Write errors:           0
    Total transferred:      418000 bytes
    HTML transferred:       32000 bytes
    Requests per second:    1059.05 [#/sec] (mean)
    Time per request:       9.442 [ms] (mean)
    Time per request:       0.944 [ms] (mean, across all concurrent requests)
    Transfer rate:          216.15 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0    9   4.1      9      33
    Processing:     0    0   0.8      0      19
    Waiting:        0    0   0.7      0      16
    Total:          3    9   4.2      9      33

    Percentage of the requests served within a certain time (ms)
      50%      9
      66%     10
      75%     11
      80%     12
      90%     14
      95%     16
      98%     21
      99%     24
     100%     33 (longest request)

Kohana 3.2.0
^^^^^^^^^^^^
.. code-block:: php

    # ab -n 2000 -c 10 http://localhost/bench/helloworld/kohana/index.php/say/hello
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/

    Benchmarking localhost (be patient)


    Server Software:        Apache/2.2.22
    Server Hostname:        localhost
    Server Port:            80

    Document Path:          /bench/helloworld/kohana/index.php/say/hello
    Document Length:        15 bytes

    Concurrency Level:      10
    Time taken for tests:   2.324 seconds
    Complete requests:      2000
    Failed requests:        0
    Write errors:           0
    Total transferred:      446446 bytes
    HTML transferred:       30030 bytes
    Requests per second:    860.59 [#/sec] (mean)
    Time per request:       11.620 [ms] (mean)
    Time per request:       1.162 [ms] (mean, across all concurrent requests)
    Transfer rate:          187.60 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0   11   5.1     10      64
    Processing:     0    0   1.9      0      39
    Waiting:        0    0   1.4      0      35
    Total:          3   11   5.3     11      64

    Percentage of the requests served within a certain time (ms)
      50%     11
      66%     13
      75%     15
      80%     15
      90%     17
      95%     18
      98%     24
      99%     31
     100%     64 (longest request)

Fuel 1.2.1
^^^^^^^^^^
.. code-block:: php

    # ab -n 2000 -c 10 http://localhost/bench/helloworld/fuel/public/say/hello
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/

    Benchmarking localhost (be patient)


    Server Software:        Apache/2.2.22
    Server Hostname:        localhost
    Server Port:            80

    Document Path:          /bench/helloworld/fuel/public/say/hello
    Document Length:        16 bytes

    Concurrency Level:      10
    Time taken for tests:   2.742 seconds
    Complete requests:      2000
    Failed requests:        0
    Write errors:           0
    Total transferred:      418000 bytes
    HTML transferred:       32000 bytes
    Requests per second:    729.42 [#/sec] (mean)
    Time per request:       13.709 [ms] (mean)
    Time per request:       1.371 [ms] (mean, across all concurrent requests)
    Transfer rate:          148.88 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0   13   6.0     12      79
    Processing:     0    0   1.3      0      22
    Waiting:        0    0   0.8      0      21
    Total:          4   14   6.1     13      80

    Percentage of the requests served within a certain time (ms)
      50%     13
      66%     15
      75%     17
      80%     17
      90%     19
      95%     24
      98%     30
      99%     38
     100%     80 (longest request)

Cake 2.1.3
^^^^^^^^^^
.. code-block:: php

    # ab -n 10 -c 5 http://localhost/bench/cake/say/hello
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/

    Benchmarking localhost (be patient).....done


    Server Software:        Apache/2.2.22
    Server Hostname:        localhost
    Server Port:            80

    Document Path:          /bench/cake/say/hello
    Document Length:        16 bytes

    Concurrency Level:      5
    Time taken for tests:   30.051 seconds
    Complete requests:      10
    Failed requests:        0
    Write errors:           0
    Total transferred:      1680 bytes
    HTML transferred:       160 bytes
    Requests per second:    0.33 [#/sec] (mean)
    Time per request:       15025.635 [ms] (mean)
    Time per request:       3005.127 [ms] (mean, across all concurrent requests)
    Transfer rate:          0.05 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0    2   3.6      0      11
    Processing: 15009 15020   9.8  15019   15040
    Waiting:        9   21   7.9     25      33
    Total:      15009 15022   8.9  15021   15040

    Percentage of the requests served within a certain time (ms)
      50%  15021
      66%  15024
      75%  15024
      80%  15032
      90%  15040
      95%  15040
      98%  15040
      99%  15040
     100%  15040 (longest request)

Zend Framework 1.11.11
^^^^^^^^^^^^^^^^^^^^^^
.. code-block:: php

    # ab -n 2000 -c 10 http://localhost/bench/helloworld/zendfw/public/index.php
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/

    Benchmarking localhost (be patient)


    Server Software:        Apache/2.2.22
    Server Hostname:        localhost
    Server Port:            80

    Document Path:          /bench/helloworld/zendfw/public/index.php
    Document Length:        16 bytes

    Concurrency Level:      10
    Time taken for tests:   5.641 seconds
    Complete requests:      2000
    Failed requests:        0
    Write errors:           0
    Total transferred:      418000 bytes
    HTML transferred:       32000 bytes
    Requests per second:    354.55 [#/sec] (mean)
    Time per request:       28.205 [ms] (mean)
    Time per request:       2.820 [ms] (mean, across all concurrent requests)
    Transfer rate:          72.36 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0   27   9.6     25      89
    Processing:     0    1   3.0      0      70
    Waiting:        0    0   2.9      0      70
    Total:          9   28   9.6     26      90

    Percentage of the requests served within a certain time (ms)
      50%     26
      66%     28
      75%     32
      80%     34
      90%     41
      95%     46
      98%     55
      99%     62
     100%     90 (longest request)

Laravel 3.2.5
^^^^^^^^^^^^^
.. code-block:: php

    # ab -n 2000 -c 10 http://localhost/bench/helloworld/laravel/public/say/hello

    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/

    Benchmarking localhost (be patient)


    Server Software:        Apache/2.2.22
    Server Hostname:        localhost
    Server Port:            80

    Document Path:          /bench/helloworld/laravel/public/say/hello
    Document Length:        15 bytes

    Concurrency Level:      10
    Time taken for tests:   4.090 seconds
    Complete requests:      2000
    Failed requests:        0
    Write errors:           0
    Total transferred:      1665162 bytes
    HTML transferred:       30045 bytes
    Requests per second:    489.03 [#/sec] (mean)
    Time per request:       20.449 [ms] (mean)
    Time per request:       2.045 [ms] (mean, across all concurrent requests)
    Transfer rate:          397.61 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0   20   7.6     19      92
    Processing:     0    0   2.5      0      53
    Waiting:        0    0   2.5      0      53
    Total:          6   20   7.6     19      93

    Percentage of the requests served within a certain time (ms)
      50%     19
      66%     21
      75%     23
      80%     24
      90%     29
      95%     34
      98%     42
      99%     48
     100%     93 (longest request)

Nette 2.0.4
^^^^^^^^^^^
.. code-block:: php

    # ab -n 2000 -c 10 http://localhost/bench/helloworld/nette/www/index.php

    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/

    Benchmarking localhost (be patient)


    Server Software:        Apache/2.2.22
    Server Hostname:        localhost
    Server Port:            80

    Document Path:          /bench/helloworld/nette/www/index.php
    Document Length:        24963 bytes

    Concurrency Level:      10
    Time taken for tests:   7.750 seconds
    Complete requests:      2000
    Failed requests:        200
       (Connect: 0, Receive: 0, Length: 200, Exceptions: 0)
    Write errors:           0
    Total transferred:      50370200 bytes
    HTML transferred:       49926200 bytes
    Requests per second:    258.07 [#/sec] (mean)
    Time per request:       38.749 [ms] (mean)
    Time per request:       3.875 [ms] (mean, across all concurrent requests)
    Transfer rate:          6347.24 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0   38  13.1     34     115
    Processing:     0    1   4.7      0      99
    Waiting:        0    0   4.5      0      98
    Total:         15   39  13.2     34     116

    Percentage of the requests served within a certain time (ms)
      50%     34
      66%     38
      75%     46
      80%     50
      90%     58
      95%     64
      98%     75
      99%     82
     100%    116 (longest request)

Phalcon Version 0.8.0
^^^^^^^^^^^^^^^^^^^^^
.. code-block:: php

    # ab -n 2000 -c 10 http://localhost/bench/helloworld/phalcon/index.php?_url=/say/hello
    This is ApacheBench, Version 2.3 <$Revision: 655654 $>
    Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
    Licensed to The Apache Software Foundation, http://www.apache.org/

    Benchmarking localhost (be patient)


    Server Software:        Apache/2.2.22
    Server Hostname:        localhost
    Server Port:            80

    Document Path:          /bench/helloworld/phalcon/index.php?_url=/say/hello
    Document Length:        16 bytes

    Concurrency Level:      10
    Time taken for tests:   0.789 seconds
    Complete requests:      2000
    Failed requests:        0
    Write errors:           0
    Total transferred:      418000 bytes
    HTML transferred:       32000 bytes
    Requests per second:    2535.82 [#/sec] (mean)
    Time per request:       3.943 [ms] (mean)
    Time per request:       0.394 [ms] (mean, across all concurrent requests)
    Transfer rate:          517.56 [Kbytes/sec] received

    Connection Times (ms)
                  min  mean[+/-sd] median   max
    Connect:        0    4   1.7      3      23
    Processing:     0    0   0.2      0       6
    Waiting:        0    0   0.2      0       6
    Total:          2    4   1.7      3      23

    Percentage of the requests served within a certain time (ms)
      50%      3
      66%      4
      75%      4
      80%      4
      90%      5
      95%      6
      98%      8
      99%     14
     100%     23 (longest request)

Gráficos
^^^^^^
O primeiro gráfico mostra quantas resuisições por segundo cada framework foi capaz de aceitar. O segundo mostra o tempo médio gasto por todas as requisições simultâneas. 

.. raw:: html

    <script type="text/javascript" src="https://www.google.com/jsapi"></script>
    <script type="text/javascript">
        google.load("visualization", "1", {packages:["corechart"]});
        google.setOnLoadCallback(drawChart);

        function drawChart() {

            var data = new google.visualization.DataTable();
            data.addColumn('string', 'Framework');
            data.addColumn('number', 'Requests per second');
            data.addRows([
                ['Nette', 258.07],
                ['Zend', 354.55],
                ['Laravel', 489.03],
                ['Symfony', 541.01],
                ['Fuel', 568.41],
                ['Yii', 851.83],
                ['Kohana', 860.59],
                ['CodeIgniter', 1059.05],
                ['Phalcon', 2535.82]
            ]);

            var options = {
                title: 'Framework / Requests per second (#/sec) [more is better]',
                colors: ['#3366CC'],
                animation: {
                    duration: 0.5
                },
                fontSize: 12,
                chartArea: {
                    width: '600px'
                }
            };

            var chart = new google.visualization.ColumnChart(document.getElementById('rps_div'));
            chart.draw(data, options);

            var data = new google.visualization.DataTable();
            data.addColumn('string', 'Framework');
            data.addColumn('number', 'Time per Request');
            data.addRows([
                ['Nette', 3.875],
                ['Zend', 2.820],
                ['Laravel', 2.045],
                ['Symfony', 1.848],
                ['Fuel', 1.371],
                ['Yii', 1.174],
                ['Kohana', 1.162],
                ['CodeIgniter', 0.944],
                ['Phalcon', 0.394]
            ]);

            var options = {
                title: 'Framework / Time per Request (mean, across all concurrent requests) [less is better]',
                colors: ['#3366CC'],
                fontSize: 11
            };

            var chart = new google.visualization.ColumnChart(document.getElementById('tpr_div'));
            chart.draw(data, options);

            var data = new google.visualization.DataTable();
            data.addColumn('string', 'Framework');
            data.addColumn('number', 'Memory Usage (MB)');
            data.addRows([
                ['Nette', 3.5],
                ['Zend', 1.75],
                ['Symfony', 1.5],
                ['Yii', 1.5],
                ['Laravel', 1.25],
                ['Kohana', 1.25],
                ['CodeIgniter', 1.1],
                ['Fuel', 1.0],
                ['Phalcon', 0.75]
            ]);

            var options = {
                title: 'Framework / Memory Usage (mean, megabytes per request) [less is better]',
                colors: ['#3366CC'],
                fontSize: 11
            };

            var chart = new google.visualization.ColumnChart(document.getElementById('mpr_div'));
            chart.draw(data, options);

            var data = new google.visualization.DataTable();
            data.addColumn('string', 'Framework');
            data.addColumn('number', 'Number of included PHP files');
            data.addRows([
                ['Zend', 66],
                ['Laravel', 46],
                ['Kohana', 46],
                ['Fuel', 30],
                ['Yii', 27],
                ['CodeIgniter', 23],
                ['Symfony', 18],
                ['Nette', 7],
                ['Phalcon', 4]
            ]);

            var options = {
                title: 'Framework / Number of included PHP files (mean, number on a single request) [less is better]',
                colors: ['#3366CC'],
                fontSize: 11
            };

            var chart = new google.visualization.ColumnChart(document.getElementById('nfi_div'));
            chart.draw(data, options);

        }
    </script>
    <div align="center">
        <div id="rps_div" style="width: 600px; height: 400px; position: relative; "><iframe name="Drawing_Frame_31166" id="Drawing_Frame_31166" width="600" height="400" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe><div></div></div>
        <div id="tpr_div" style="width: 600px; height: 400px; position: relative; "><iframe name="Drawing_Frame_89467" id="Drawing_Frame_89467" width="600" height="400" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe><div></div></div>
        <div id="nfi_div" style="width: 600px; height: 400px; position: relative; "><iframe name="Drawing_Frame_49746" id="Drawing_Frame_49746" width="600" height="400" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe><div></div></div>
        <div id="mpr_div" style="width: 600px; height: 400px; position: relative; "><iframe name="Drawing_Frame_77939" id="Drawing_Frame_77939" width="600" height="400" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe><div></div></div>
    </div>

Conclusão
----------
A compilação natural do Phalcon oferece extraordinário desempenho que supera todos os outros frameworks mensurados nesses benchmarks.


.. _get_included_files: http://www.php.net/manual/en/function.get-included-files.php
.. _memory_get_usage: http://php.net/manual/en/function.memory-get-usage.php
.. _Yii: http://www.yiiframework.com/
.. _Symfony: http://symfony.com/
.. _CodeIgniter: http://codeigniter.com/
.. _Kohana: http://kohanaframework.org/index
.. _FuelPHP: http://fuelphp.com/
.. _CakePHP: http://cakephp.org/
.. _Laravel: http://www.laravel.com/
.. _Zend Framework: http://framework.zend.com
.. _Nette: http://nette.org/

