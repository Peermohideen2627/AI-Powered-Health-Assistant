     //Backend (Flask API)

from flask import Flask, request, jsonify  
import symptom_checker  # Custom ML model  

app = Flask(__name__)  

@app.route('/check_symptoms', methods=['POST'])  
def check_symptoms():  
    data = request.json  
    symptoms = data.get("symptoms", [])  
    diagnosis = symptom_checker.analyze(symptoms)  
    return jsonify({"diagnosis": diagnosis})  

if __name__ == '__main__':  
    app.run(debug=True)

    //Frontend (React Native)

import React, { useState } from 'react';  
import { View, Text, TextInput, Button } from 'react-native';  

export default function App() {  
  const [symptoms, setSymptoms] = useState("");  
  const [result, setResult] = useState("");  

  const checkHealth = async () => {  
    const response = await fetch('http://localhost:5000/check_symptoms', {  
      method: 'POST',  
      headers: { 'Content-Type': 'application/json' },  
      body: JSON.stringify({ symptoms: symptoms.split(",") })  
    });  
    const data = await response.json();  
    setResult(data.diagnosis);  
  };  

  return (  
    <View>  
      <Text>Enter Symptoms:</Text>  
      <TextInput onChangeText={setSymptoms} />  
      <Button title="Check Health" onPress={checkHealth} />  
      <Text>Result: {result}</Text>  
    </View>  
  );  
}


    
