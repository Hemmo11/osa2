import { useState } from 'react'

const PersonForm = (props) => {
  return (
    <form onSubmit={props.onSubmit}>
    <div>
      name: <input value={props.newName} onChange={props.handleNameChange} />
    </div>
    <div>
      number: <input value={props.newNumber} onChange={props.handleNumberChange} />
    </div>
     <br />
     <button type="submit">add</button>
     <br/>
  </form>
  )
}
const App = () => {
  const [persons, setPersons] = useState([
    { name: 'Arto Hellas', number: '040-123456', id: 1 },
    { name: 'Ada Lovelace', number: '39-44-5323523', id: 2 },
    { name: 'Dan Abramov', number: '12-43-234345', id: 3 },
    { name: 'Mary Poppendieck', number: '39-23-6423122', id: 4 }
  ])

  const [newName, setNewName] = useState('')

  const [newNumber, setNewNumber] = useState('')

  const handleNameChange = (event) => {
    setNewName(event.target.value)
  }
  const handleNumberChange = (event) => {
    setNewNumber(event.target.value)
  }
  const addPerson = (event) => {
    event.preventDefault()

    const newPerson = { 
      name: newName, 
      number: newNumber,
      id: persons.length + 1 }

    if (!persons.some(person => person.name === newName)) {
      setPersons(persons.concat(newPerson))
    }
    else alert(`${newName} is already added to phonebook`)
    
    setNewName('') // clear input
    setNewNumber('') // clear input
  }
const [filter, setFilter] = useState('')

const handleFilterChange = (event) => {
  setFilter(event.target.value)
}
const personsToShow = persons.filter(person => person.name.toLowerCase().includes(filter.toLowerCase()))


  return (
    <div>
      <h2>Phonebook</h2>
      <div>
      filter shown with <input value={filter} onChange={handleFilterChange} />
    </div>
      <br />
      <h2>add a new</h2>
      <PersonForm 
      onSubmit={addPerson}
      newName={newName}
      handleNameChange={handleNameChange}
      newNumber={newNumber}
      handleNumberChange={handleNumberChange}
      />
      <h2>Numbers</h2>
      <div>
        {personsToShow.map(person => (
          <p key={person.name}>
            {person.name} {person.number}
          </p>
        ))}
      </div>
    </div>
  )
}

export default App
