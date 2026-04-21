import { useState, useEffect } from 'react'
import axios from 'axios'

const App = () => {
  const [countries, setCountries] = useState([])
  const [filter, setFilter] = useState('')

  useEffect(() => {
    axios.get('https://studies.cs.helsinki.fi/restcountries/api/all')
      .then(response => setCountries(response.data))
  }, [])

  const filtered = countries.filter(c =>
    c.name.common.toLowerCase().includes(filter.toLowerCase())
  )

  let content = null

  if (filtered.length > 10) {
    content = <div>Too many matches, specify another filter</div>

  } else if (filtered.length > 1) {
    content = (
      <ul>
        {filtered.map(c =>
          <li key={c.cca3}>{c.name.common}</li>
        )}
      </ul>
    )

  } else if (filtered.length === 1) {
    const country = filtered[0]
    content = (
      <div>
        <h2>{country.name.common}</h2>
        <p>capital: {country.capital}</p>
        <p>area: {country.area}</p>
      </div>
    )
  }

  return (
    <div>
      <p>find countries:</p>

      <input
        value={filter}
        onChange={(e) => setFilter(e.target.value)}
      />

      <h2>Countries</h2>

      {content}
    </div>
  )
}

export default App