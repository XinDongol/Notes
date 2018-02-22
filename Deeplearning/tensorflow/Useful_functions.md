### Tensor Collection 

tf.add\_to\_collection：把变量放入一个集合，把很多变量变成一个列表

tf.get\_collection：从一个结合中取出全部变量，是一个列表

tf.add\_n：把一个列表的东西都依次加起来

例如：

```Python 
import tensorflow as tf;  
import numpy as np;  
import matplotlib.pyplot as plt;  

v1 = tf.get_variable(name='v1', shape=[1], initializer=tf.constant_initializer(0))
tf.add_to_collection('loss', v1)
v2 = tf.get_variable(name='v2', shape=[1], initializer=tf.constant_initializer(2))
tf.add_to_collection('loss', v2)

with tf.Session() as sess:
	sess.run(tf.initialize_all_variables())
	print tf.get_collection('loss')
	print sess.run(tf.add_n(tf.get_collection('loss')))

```

### Monitoring via Tensorboard

#####Add Histogram Monitor 
	tf.summary.histogram('sigma',tf.reshape(tf.exp(0.5*log_sigma2),[-1]))
	
#####Add Scalar Monitor
	tf.summary.scalar('cross_entropy',cross_entropy) 
	
#####Merge Monitors
	merged_summary_op = tf.summary.merge_all()
	
#####Set Log-dir
	logs_path = '/home/xdong/codes/tf-variational-dropout-master/logs_path/exp0/'
	summary_writer = tf.summary.FileWriter(logs_path)
	
#####At Each Step
	summary_writer.add_summary(summary, global_step=int(i/1000))
	
#####Lunch Tensorboard
	tensorboard --logdir=logs_path	
	