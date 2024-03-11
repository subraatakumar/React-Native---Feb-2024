# AsyncStorage

```js
// Custom Hook : useAsyncStorage
import {useState, useEffect} from 'react';
import AsyncStorage from '@react-native-async-storage/async-storage';

const isJSON = str => {
  try {
    JSON.parse(str);
  } catch (e) {
    return false;
  }
  return true;
};

const useAsyncStorage = key => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState('');

  useEffect(() => {
    const getData = async () => {
      try {
        const data = await AsyncStorage.getItem(key);
        data && isJSON(data) ? setData(JSON.parse(data)) : setData(data || '');
      } catch (err) {
        setError(err);
      } finally {
        setLoading(false);
      }
    };

    getData();
  }, [key]); // Re-run only if `key` changes

  const setValue = async value => {
    try {
      const stringValue =
        typeof value == 'string' ? value : JSON.stringify(value); // Stringify for storage
      await AsyncStorage.setItem(key, stringValue);
      setData(value); // Update local state for immediate feedback
    } catch (err) {
      setError(err);
    }
  };

  return {data, loading, error, setValue};
};

export default useAsyncStorage;
```

```js
// App.js
import {Button, Text, View} from 'react-native';
import useAsyncStorage from './src/customHook/useAsyncStorage';

const App = () => {
  const {data, loading, error, setValue} = useAsyncStorage('myDataKey');

  // Handle loading, error, and data states
  if (loading) return <Text>Loading...</Text>;
  if (error.trim().length != 0) return <Text>Error: {error}</Text>;

  // Use the data
  return (
    <View>
      <Text>Data: {data}</Text>
      <Button title="Set Data" onPress={() => setValue('New Data!')} />
    </View>
  );
};

export default App;
```

# Realm

- Install Realm

```js
yarn add realm @realm/react
```

- Defining the Task Model

```js
// Task.js
import Realm from 'realm';

class Task extends Realm.Object {}

Task.schema = {
  name: 'Task',
  primaryKey: '_id',
  properties: {
    _id: 'objectId',
    title: {type: 'string'},
    completed: {type: 'bool'},
  },
};

export default Task;
```

- Write the main App

```js
import React, {useState, useEffect} from 'react';
import {View, Button, Text, TouchableOpacity, TextInput} from 'react-native';
import Realm from 'realm';
import Task from './Task'; // Assuming Task.js is in the same directory

const App = () => {
  const [taskInput, setTaskInput] = useState('');
  const [tasks, setTasks] = useState([]);

  useEffect(() => {
    const config = {
      schema: [Task], // Add your models here
    };

    Realm.open(config)
      .then(realm => {
        console.log('Realm opened successfully');
        refreshTasks(realm); // Fetch tasks on initial load
      })
      .catch(error => {
        console.error('Failed to open Realm:', error);
      });
  }, []);

  const addTask = (title: string) => {
    Realm.open({schema: [Task]})
      .then(realm => {
        realm.write(() => {
          realm.create(Task, {
            _id: new Realm.BSON.ObjectId(),
            title,
            completed: false,
          });
        });
        refreshTasks(realm); // Update tasks state after adding
      })
      .catch(error => {
        console.error('Failed to add task:', error);
      });
  };

  const deleteTask = (taskId: typeof Realm.BSON.ObjectId) => {
    Realm.open({schema: [Task]})
      .then(realm => {
        realm.write(() => {
          const taskToDelete = realm.objectForPrimaryKey('Task', taskId); // Find task by ID
          if (taskToDelete) {
            realm.delete(taskToDelete);
          } else {
            console.warn(`Task with ID "${taskId}" not found in Realm.`);
          }
        });
        refreshTasks(realm); // Update tasks state after deletion
      })
      .catch(error => {
        console.error('Failed to delete task:', error);
      });
  };

  const refreshTasks = (realm: Realm) => {
    const allTasks = realm.objects('Task'); // Get all Task objects
    setTasks(allTasks);
  };

  return (
    <View>
      <TextInput
        value={taskInput}
        onChangeText={setTaskInput}
        placeholder="Input Task"
        style={{borderWidth: 1, borderColor: 'rgba(0,0,0,0.5)'}}
      />
      <Button
        title="Add Task"
        onPress={() => {
          addTask(taskInput.trim().length > 0 ? taskInput : 'New Task');
          setTaskInput('');
        }}
      />
      {tasks.map(task => (
        <View style={{flexDirection: 'row', justifyContent: 'space-between'}}>
          <Text key={task.id}>
            {task.title} - {task.completed ? 'Completed' : 'Pending'}
          </Text>
          <TouchableOpacity onPress={() => deleteTask(task['_id'])}>
            <Text>Delete</Text>
          </TouchableOpacity>
        </View>
      ))}
    </View>
  );
};

export default App;
```

