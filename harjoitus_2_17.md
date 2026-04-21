
import { useState, useEffect } from 'react'
import personService from './services/persons'

const Notification = ({ message }) => {
  if (!message) return null

  return (
    <div className={`notification ${message.type}`}>
      {message.text}
    </div>
  )
}

const App = () => {
  const [persons, setPersons] = useState([])
  const [newName, setNewName] = useState('')
  const [newNumber, setNewNumber] = useState('')
  const [filter, setFilter] = useState('')
  const [message, setMessage] = useState(null)

  useEffect(() => {
    personService.getAll().then(data => {
      setPersons(data)
    })
  }, [])
 
const addPerson = (e) => {
  e.preventDefault()

  const newPerson = {
    name: newName,
    number: newNumber
  }

  const person = persons.find(p => p.name === newName)

  if (!person) {
    personService.create(newPerson).then(data => {
      setPersons(prev => prev.concat(data))
      setMessage({
        text: `Added ${data.name}`,
        type: 'success'
      })
      setTimeout(() => {setMessage(null)}, 5000)

      setNewName('')
      setNewNumber('')
    })
  } else {

    if (window.confirm(`${newName} is already added. Replace number?`)) {

      const changedPerson = { ...person, number: newNumber }

      personService.update(person.id, changedPerson)
        .then(returned => {
        setPersons(prev =>
        prev.map(p =>
        p.id !== person.id ? p : returned
    )
  )
        
          setNewName('')
          setNewNumber('')
        })
        .catch (error => {
          setMessage({
            text: `Information of ${person.name} has already been removed from server`,
            type: 'error'
          })
          setTimeout(() => {setMessage(null)}, 5000)

          setPersons(persons.filter(p => p.id !== person.id))
        })
        
    }
  }
}

  const deletePerson = (id) => {
    const person = persons.find(p => p.id === id)

    if (window.confirm(`Delete ${person.name}?`)) {
      personService.remove(id).then(() => {
        setPersons(persons.filter(p => p.id !== id))
        setMessage({
          text: `Deleted ${person.name}`,
          type: 'success'
        })
        setTimeout(() => {setMessage(null)}, 5000)
      })
    }
  }

  const personsToShow = persons.filter(p =>
    p.name.toLowerCase().includes(filter.toLowerCase())
  )

  return (
    <div>
      <h2>Phonebook</h2>

      <Notification message={message} />

      <input
        value={filter}
        onChange={(e) => setFilter(e.target.value)}
        placeholder="filter"
      />

      <h2>Add</h2>
      
      <form onSubmit={addPerson}>
       <div>
        name: <input value={newName} onChange={(e) => setNewName(e.target.value)} />
       </div>
        number: <input value={newNumber} onChange={(e) => setNewNumber(e.target.value)} />
        <button type="submit">add</button>
      </form>

      <h2>Numbers</h2>

      {personsToShow.map(p => (
        <div key={p.id}>
          {p.name} {p.number}
          <button onClick={() => deletePerson(p.id)}>delete</button>
        </div>
      ))}
    </div>
  )
}

export default App