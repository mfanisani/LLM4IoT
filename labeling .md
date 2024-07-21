# This prompt is responsible to provide labels for sensor updates. It has two chatCompletion messages. We used Jinja templates. Note that we pass the information in the placeholders indicated in {{ }}.
# system message
label_temp_system_message =Template("""You are a powerful tool to provide lalbels that are activities if they happen in the house.
We have some sensors that will be shared with you. However, you will recieve only the values for each sensor as 0 or 1. SensorStates provides the aggreagated values.
                          
The senors and the way you will underestand them are {{sensors_with_descripion}}

As a training data consider the following data
- Note that this example may not have all the labels.
- The following example shows the expected labeling for one day.
{{one_day_labeled_example}}
------------                            
You will recive some data of the sensors of the previous times.
The user will ask for specific row and you should provide that information.
The expected format is as follows:
<reasoning> /// why you provide the label </reasoning>
<label> /// the label </label>
Now You will receive two examples of this task
Example_0:
user:The previous data of the sensors:
                             8:50:49	10000000000001000	
 8:53:39	10000000010001000	
 8:53:44	10000000000001000	
 8:59:25	10001000000001000	toilet begin
 9:02:28	10000000000001000	toilet end
 9:03:56	10000100000001000	washing face begin
 9:05:40	10000000000001000	washing face end
 9:06:04	10000000000000000	meal preparation end
 9:06:09	10000000010000000	
 9:06:16	10000000000000000	
 9:08:20	10000000010000000	
 9:08:30	10000000000000000	
Now give me the label of the row 9:10:38	10100000000000000
assistant: 
<reasoning> The SensorStates is changing from 10000000000000000 to 10100000000000000. </reasoning>
<label> watching tv begin </label>

                            

Example_1:
user:The previous data of the sensors:
9:10:38	10100000000000000	watching tv begin
9:18:52	00000000000000000	watching tv end
9:22:56	00000000000000010	
9:22:57	00000000000000000	
9:22:59	00000000000000010	
9:23:33	00000000000000000	
9:23:54	00000000000000010	
9:24:48	00000000000000000	
9:33:32	00001000000000000	toilet begin
9:35:14	00000000000000000	toilet end
9:43:11	00000000000000010	
9:43:29	00000000000000000	
9:43:31	00000000000000010	
9:45:34	00000000000000000	
9:45:37	00000000000000010	
9:54:07	00000000000000000	
9:54:45	00001000000000000	toilet begin

Now give me the label of the row 9:58:11	00000000000000000

assistant:
<reasoning> The SensorStates is changing from 00001000000000000 to 00000000000000000. </reasoning>
<label> toilet end </label>

Example_2:
user:The previous data of the sensors:
23:37:25	00000000000000000	
23:37:28	00000000000000010	
23:37:54	00000000000000000	
0:49:38	10000000000000000	
0:50:20	10000000010000000	
Now give me the label of the row 0:50:29	10000000000000000	
assistant:
<reasoning> The SensorStates is changing from 10000000010000000 to 10000000000000000. </reasoning>
<label> (blank) </label>
---------
system:                                                                                 
Now the user will give you some previous rows and you should provide the label of a specific row.
Please first explain the change in SensorStates in 20 words in <reasoning> tag, find the pattern and then provide the label in <label> tag.
""")
# system message
label_temp_user_message =Template("""You should give me the label. The possible labels are as follows:
going to outside	meal begin	meal end	meal preparation begin	meal preparation end	prayering begin	prayering end	relax begin	relax end	return home	sleeping begin	sleeping end	telephone begin	telephone end	toilet begin	toilet end	washing face begin	washing face end	washing machine begin	watching tv begin	watching tv end	(blank)
You are not allowed to use any other label.
Note that the SensorStates is the aggregation of the sensors. The sensors are as follows:
{{sensors}}
The previous data of the sensors:
{{previous_sensors}}
Now give me the label of the row {{current_sensors_values}}                                                                              
