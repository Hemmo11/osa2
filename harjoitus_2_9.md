import { useState } from 'react'

const App = () => {
  const [persons, setPersons] = useState([
    { name: 'Arto Hellas' }
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

    const newPerson = { name: newName, number: newNumber }

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
      <br />
      Filter shown with <input value={filter} onChange={handleFilterChange} />
      <br />
      <h2>add a new</h2>
      <form onSubmit={addPerson}>
        <div>
          name: <input value={newName} onChange={handleNameChange} />
          <div>
            number: <input value={newNumber} onChange={handleNumberChange} /></div>
        </div>
        <div>
          <button type="submit">add</button>
        </div>
      </form>

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
