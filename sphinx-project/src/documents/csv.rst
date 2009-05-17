PyMOTW: csv
====================

.. currentmodule:: csv

* ģ��: csv
* Ŀ��: ���Էֺŷָ�����ֵ�ļ����ж�д
* Python �汾: 2.3+

����:
---------

csv ģ���ڴ�����Щ�ӵ������ݱ������ݿ��е��뵽�ı��ļ�������ʱ, �Ǻ����õ�. ���ﲢû�кܺõĶ����׼, ���csvģ��ʹ����"dialects", ͨ��ʹ�ò�ͬ�Ĳ���������csv�ļ�. ����һ��Ķ���д, ���ģ��Ҳ�ܴ���Microsoft Excel��ʽ����.

������:
----------

Python 2.5 �汾��csv��֧��unicode����, ������ASCII��NUL�ַ�����Ҳ�е�����, �����Ƽ�ʹ��UTF-8��ɴ�ӡASCII�ַ�.

��ȡ:
----------

��csv�ļ��ж�ȡ����, ����ʹ��reader()����������һ����ȡ����. �����ȡ����˳�����ļ���ÿһ��, ���԰������ɵ�����ʹ��, ����:

.. code-block::python

    import csv
    import sys

    f = open(sys.argv[1], 'rt')
    try:

        reader = csv.reader(f)
        for row in reader:
        print row

    finally:

        f.close()


reader()�ĵ�һ������ָʾԴ�ı���, �����������, ��һ���ļ�, �����������κο�ת���Ķ���(StringIO����, lists��). ָ��������ѡ�Ĳ��������ڿ��������������α�����.

�����ļ�"testdata.csv"�Ǵ�NeoOffice�е����, ����������.

::

    $ cat testdata.csv 
    "Title 1","Title 2","Title 3"
    1,"a",08/18/07
    2,"b",08/19/07
    3,"c",08/20/07
    4,"d",08/21/07
    5,"e",08/22/07
    6,"f",08/23/07
    7,"g",08/24/07
    8,"h",08/25/07
    9,"i",08/26/07

������ȡʱ, �������ݵ�ÿһ�б�ת��Ϊһ���ַ����б�.

::

    $ python csv_reader.py testdata.csv
    ['Title 1', 'Title 2', 'Title 3']
    ['1', 'a', '08/18/07']
    ['2', 'b', '08/19/07']
    ['3', 'c', '08/20/07']
    ['4', 'd', '08/21/07']
    ['5', 'e', '08/22/07']
    ['6', 'f', '08/23/07']
    ['7', 'g', '08/24/07']
    ['8', 'h', '08/25/07']
    ['9', 'i', '08/26/07']


�����֪���ض����о����ض�������, ��Ϳ�������ת��, ��csv�����Զ�ת��. �����Զ�����Ƕ����һ���ַ�����(����к�����Դ�ļ���"��"��˼�ǲ�ͬ��)�Ļ��з�.

::

    $ cat testlinebreak.csv 
    "Title 1","Title 2","Title 3"
    1,"first line ## ����Դ�ļ���һ��line
    second line",08/18/07

    $ python csv_reader.py testlinebreak.csv 
    ['Title 1', 'Title 2', 'Title 3']
    ['1', 'first line\nsecond line', '08/18/07'] ## ����csv��һ��row


д��:
---------

����������ݵ��뵽����Ӧ�ó�����, ��CSV�ļ���д��Ҳ�Ƿǳ������. ʹ��writer()����������һ��д�����, ����ÿһ��, ʹ��writerow()�����һ��.

.. code-block::python

    import csv
    import sys

    f = open(sys.argv[1], 'wt')
    try:

        writer = csv.writer(f)
        writer.writerow( ('Title 1', 'Title 2', 'Title 3') )
        for i in range(10):
        writer.writerow( (i+1, chr(ord('a') + i), '08/%02d/07' % (i+1)) )

    finally:

        f.close()

������ӵ������������ȡ���ӵĵ������ݿ���������ôһ��.

::
    $ python csv_writer.py testout.csv 
    $ cat testout.csv 
    Title 1,Title 2,Title 3
    1,a,08/01/07
    2,b,08/02/07
    3,c,08/03/07
    4,d,08/04/07
    5,e,08/05/07
    6,f,08/06/07
    7,g,08/07/07
    8,h,08/08/07
    9,i,08/09/07
    10,j,08/10/07

д�����û��ʹ��Ĭ�ϵ�����, ����ÿ���ַ���û��������������. ��������Ӷ�������ò������ɽ�����ֵ����������������.

.. code-block::python

    writer = csv.writer(f, quoting=csv.QUOTE_NONNUMERIC)


����ÿ���ַ���������������:

::

    $ python csv_writer_quoted.py testout_quoted.csv 
    $ cat testout_quoted.csv 
    "Title 1","Title 2","Title 3"
    1,"a","08/01/07"
    2,"b","08/02/07"
    3,"c","08/03/07"
    4,"d","08/04/07"
    5,"e","08/05/07"
    6,"f","08/06/07"
    7,"g","08/07/07"
    8,"h","08/08/07"
    9,"i","08/09/07"
    10,"j","08/10/07"


����:
--------

����4�ֲ�ͬ������ѡ��, ������Ϊ����������csvģ����.

QUOTE_ALL
    ������ʲô����, �κ����ݶ���������

QUOTE_MINIMAL
    ����Ĭ�ϵ�, ʹ��ָ�����ַ����ø�����(���������������Ϊ��ͬ��dialect��ѡ��ʱ, ���ܻ��ý������ڽ���ʱ��������)

QUOTE_NONNUMERIC
    ������Щ���������򸡵�������. ��ʹ�ö�ȡ����ʱ, ������������û������, ��ô���ǻᱻת���ɸ�����.

QUOTE_NONE
    �����е�������ݶ���������, ��ʹ�ö�ȡ����ʱ, �����ַ������ǰ�����ÿ�����ֵ��(�������������, ���Ǳ����ɶ��������ȥ��)


Dialects:
------------

�кܶ�������Կ���csvģ����ν������ȡ����. ���ⲻ��ͨ�����Դ��ݸ���ȡ�����д�������ز���, ����ͳһ����, ʹ��һ��"dialect"����. Dialect�����ͨ������ע��, ���csvģ�������ʱ���Բ���Ԥ��֪����صĲ�������. ��׼���������dialects: excel��excel-tabs. "excel" dialect�����ڴ���Ĭ������ Microsoft Excel��ʽ�����ݵ�, ͬ��, Ҳ���Դ��� OpenOffice �� NeoOffice������. ������ϸ��dialect��������ʹ����csvģ��� `��9.1.2 <http://docs.python.org/lib/csv-fmt-params.html>`_ ����˵��.      ## dialect����һЩ����(�����, ���з��ȵ�)����, Ԥ�����úõ�, ��ͬ������Ҳ�����Լ��趨,

DictReader ��DictWriter:
---------------------------

����, �ڴ�����������ʱ, csvģ�������һЩ������Ϊ�ֵ���д������. ��DictReader����DictWriter��ÿһ��ת���ֵ����, ���Դ����ֵ��ֵ, ���ߴ������ļ��ĵ�һ�����ƶϳ���ֵ.

.. code-block:: python

    import csv
    import sys

    f = open(sys.argv[1], 'rt')
    try:

        reader = csv.DictReader(f)
        for row in reader:
        print row

    finally:
         f.close()


�����ֵ�Ķ�ȡ��д�������Ե����ǻ������ж���Ľ�һ��ʵ��, ����ʹ����ͬ�Ĳ�����API. Ψһ�Ĳ�����ǰ�߰�ÿһ�е������ֵ�������б��Ԫ��.

::

    $ python csv_dictreader.py testdata.csv 
    {'Title 1': '1', 'Title 3': '08/18/07', 'Title 2': 'a'}
    {'Title 1': '2', 'Title 3': '08/19/07', 'Title 2': 'b'}
    {'Title 1': '3', 'Title 3': '08/20/07', 'Title 2': 'c'}
    {'Title 1': '4', 'Title 3': '08/21/07', 'Title 2': 'd'}
    {'Title 1': '5', 'Title 3': '08/22/07', 'Title 2': 'e'}
    {'Title 1': '6', 'Title 3': '08/23/07', 'Title 2': 'f'}
    {'Title 1': '7', 'Title 3': '08/24/07', 'Title 2': 'g'}
    {'Title 1': '8', 'Title 3': '08/25/07', 'Title 2': 'h'}
    {'Title 1': '9', 'Title 3': '08/26/07', 'Title 2': 'i'}



DictWriter����ָ��һ�������ֵ��б�, ��Ϊ�������������ʱ֪��ÿ���е�˳��.

.. code-block:: python

    import csv
    import sys

    f = open(sys.argv[1], 'wt')
    try:

        fieldnames = ('Title 1', 'Title 2', 'Title 3')
        writer = csv.DictWriter(f, fieldnames=fieldnames)
        headers = {}
        for n in fieldnames:
            headers[n] = n
        writer.writerow(headers)
        for i in range(10):
            writer.writerow({ 'Title 1':i+1,
                            'Title 2':chr(ord('a') + i),
                            'Title 3':'08/%02d/07' % (i+1),
                            })

    finally:

        f.close()

::

    $ python csv_dictwriter.py testout.csv 
    $ cat testout.csv 
    Title 1,Title 2,Title 3
    1,a,08/01/07
    2,b,08/02/07
    3,c,08/03/07
    4,d,08/04/07
    5,e,08/05/07
    6,f,08/06/07
    7,g,08/07/07
    8,h,08/08/07
    9,i,08/09/07
    10,j,08/10/07


�ο�:
-------
* `Python Module of the Week Home <http://www.doughellmann.com/projects/PyMOTW/>`_
* `Download Sample Code <http://www.doughellmann.com/downloads/PyMOTW-1.14.tar.gz>`_
* `PEP 305, CSV File API <http://www.python.org/peps/pep-0305.html>`_
