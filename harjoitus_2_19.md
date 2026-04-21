import { useState, useEffect } from 'react'
import axios from 'axios'

const App = () => {
  const [countries, setCountries] = useState([])
  const [filter, setFilter] = useState('')
  const [selected, setSelected] = useState(null)

  useEffect(() => {
    axios.get('https://studies.cs.helsinki.fi/restcountries/api/all')
      .then(response => setCountries(response.data))
  }, [])

  const filtered = countries.filter(c =>
    c.name.common.toLowerCase().includes(filter.toLowerCase())
  )

  // kun filter muuttuu → sulje detail view
  useEffect(() => {
    setSelected(null)
  }, [filter])

  // LISTA + CONTENT LOGIC
  let content = null

  if (filtered.length > 10) {
    content = <div>Too many matches, specify another filter</div>

  } else if (filtered.length > 1) {
    content = (
      <ul>
        {filtered.map(c =>
          <li key={c.cca3}>
            {c.name.common}{' '}
            <button onClick={() => setSelected(c)}>
              show
            </button>
          </li>
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

  // DETAIL VIEW (Show-nappi)
  let countryDetail = null

  if (selected) {
    countryDetail = (
      <div>
        <h2>{selected.name.common}</h2>
        <p>capital: {selected.capital}</p>
        <p>area: {selected.area}</p>

        <button onClick={() => setSelected(null)}>
          back
        </button>
      </div>
    )
  }

  // RENDER
  return (
    <div>
      <p>find countries:</p>

      <input
        value={filter}
        onChange={(e) => setFilter(e.target.value)}
      />

      {/* jos maa valittu → näytä se, muuten lista */}
      {selected ? countryDetail : content}
    </div>
  )
}

export default App