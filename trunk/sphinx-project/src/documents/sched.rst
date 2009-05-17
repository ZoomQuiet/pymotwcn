PyMOTW: sched
========================

.. currentmodule:: sched

schedģ��ʵ����һ���¼����ȹ���, ����ָ��ʱ��ִ��ĳ������.

* ģ��: sched
* Ŀ��: һ���¼�����.
* Python �汾: 1.4 + 

����:
------

scheduler��ʹ��һ����¼����Ƚӿ�. ��ʹ��time��������õ�ǰʱ��, delay�������ڵȴ�һ���ض�ʱ��. ����, ����ʹ��ʲô����ʱ�䵥λ���Ǻ���Ҫ, ��Ϊ�����ýӿڸ��������, �����ڶ�����;.

time��������ʱ����Ҫ�����κβ���, Ӧ���ص�ǰʱ����ַ�����ʾ. ��delay������Ҫһ�����Ͳ���, ��time����ʹ����ͬ��ʱ��̶�, �ú����ڷ���ǰ��Ҫ�ȴ��ض���ʱ�䵥Ԫ. ����, time.time()��time.sleep()����������������ЩҪ��.

Ϊ��֧�ֶ��߳�Ӧ��, ������ÿ���߳�֮��, ���ò���Ϊ0��delay����, ��������֤�����߳��л�������.


�ӳٺ������¼�:
--------------------

�¼��������ӳ�һ��ʱ���, ����ָ��ʱ����ϵ���ִ��.  enter()����ʹ��Щ�¼����ӳ�һ��ʱ��󱻵���, ����Ҫ4������:

 * A number representing the delay �����ӳٶ೤ʱ�������
 * A priority value ���ȼ�ֵ
 * The function to call ��Ҫ�����õĺ���
 * A tuple of arguments for the function �����Ĳ���Ԫ��


�������������, �ֱ���2��3��֮�����2����ͬ���¼�. ������ĳ�¼��ĵ���ʱ��, print_event()������, ��ʾ��Ŀǰʱ��ʹ��ݸ��¼��Ĳ�������.

.. code-block:: python

    import sched
    import time

    scheduler = sched.scheduler(time.time, time.sleep)

    def print_event(name):
        print 'EVENT:', time.time(), name

    print 'START:', time.time()
    scheduler.enter(2, 1, print_event, ('first',))
    scheduler.enter(3, 1, print_event, ('second',))

    scheduler.run()

�������:

::

    $ python sched_basic.py
    START: 1190727943.36
    EVENT: 1190727945.36 first
    EVENT: 1190727946.36 second

��һ���¼���ʱ����Ϣ�ǵ��ȿ�ʼ2���, �ڶ����¼���ʱ����Ϣ�ǵ��ȿ�ʼ3���.

�¼��ص�:
-------------

run()һֱ������, ֱ�������¼���ȫ��ִ����. ÿ���¼���ͬһ�߳�������, �������һ���¼���ִ��ʱ����������¼����ӳ�ʱ��, ��ô, �ͻ�����ص�. �ص��Ľ���������Ƴٺ����¼���ִ��ʱ��. ������֤û�ж�ʧ�κ��¼�, ����Щ�¼��ĵ���ʱ�̻��ԭ���趨�ĳ�. �������������, long_event()��ͨ��˯��2�������ӳٵ���, ͬ���ӳٵ��Ⱥ�����ͨ�����г�ʱ����������I/O��ʵ��.

.. code-block:: python

    import sched
    import time

    scheduler = sched.scheduler(time.time, time.sleep)

    def long_event(name):
        print 'BEGIN EVENT :', time.time(), name
        time.sleep(2)
        print 'FINISH EVENT:', time.time(), name

    print 'START:', time.time()
    scheduler.enter(2, 1, long_event, ('first',))
    scheduler.enter(3, 1, long_event, ('second',))

    scheduler.run()

�ڶ����¼��ڵ�һ���¼����н�������������, ��Ϊ��һ���¼���ִ��ʱ���㹻��, �Ѿ������ڶ����¼���Ԥ�ڿ�ʼʱ��.

::

    $ python sched_overlap.py 
    START: 1190728573.16
    BEGIN EVENT : 1190728575.16 first
    FINISH EVENT: 1190728577.16 first
    BEGIN EVENT : 1190728577.16 second
    FINISH EVENT: 1190728579.16 second


�¼����ȼ�:
---------------

�������ͬ��ʱ�̵����ж���¼���Ҫ��ִ��, ��ô���ǵ����ȼ������������ǵ�ִ��˳��.

.. code-block:: python

    now = time.time()
    print 'START:', now
    scheduler.enterabs(now+2, 2, print_event, ('first',))
    scheduler.enterabs(now+2, 1, print_event, ('second',))
    scheduler.run()

Ϊ�˱�֤�¼�׼ȷ����ͬһʱ��ִ��, ʹ����enterabs()����������enter()����. enterabs()�ĵ�һ�������������¼���ȷ��ʱ��, �������ӳ�ʱ����.

::

    $ python sched_priority.py 
    START: 1190728789.4
    EVENT: 1190728791.4 second
    EVENT: 1190728791.4 first


ȡ���¼�:
--------------

enter()��enterabs()����һ�¼�������, �����ÿɱ������¼���ȡ��. ����run()����, �����¼���ȡ��������Ҫ������һ���߳��н���. ��������, ��һ�����߳̿�ʼִ�е���, ���������߳�����ȡ��ĳ���¼�.

.. code-block:: python

    import sched
    import threading
    import time

    scheduler = sched.scheduler(time.time, time.sleep)

    # Set up a global to be modified by the threads
    counter = 0

    def increment_counter(name):

        global counter
        print 'EVENT:', time.time(), name
        counter += 1
        print 'NOW:', counter


    print 'START:', time.time()
    e1 = scheduler.enter(2, 1, increment_counter, ('E1',))
    e2 = scheduler.enter(3, 1, increment_counter, ('E2',))

    # Start a thread to run the events
    t = threading.Thread(target=scheduler.run)
    t.start()

    # Back in the main thread, cancel the first scheduled event.
    scheduler.cancel(e1)

    # Wait for the scheduler to finish running in the thread
    t.join()

    print 'FINAL:', counter


�����¼������ŵ���, ��֮��ȡ���˵�һ���¼�. ֻ�еڶ����¼�ִ����, �������ǿ������������ۼ���һ��.

::
    $ python sched_cancel.py
    START: 1190729094.13
    EVENT: 1190729097.13 E2
    NOW: 1
    FINAL: 1


�ο�:
-------

* `Python Module of the Week Home <http://www.doughellmann.com/projects/PyMOTW/>`_
* `Download Sample Code <http://www.doughellmann.com/downloads/PyMOTW-1.19.tar.gz>`_
