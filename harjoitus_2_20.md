import { useState, useEffect } from 'react'
import axios from 'axios'

const App = () => {
  const [countries, setCountries] = useState([])
  const [filter, setFilter] = useState('')
  const [selected, setSelected] = useState(null)
  const [weather,setWeather]=useState(null)

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
  
  useEffect(() => {
  if (!selected) return

  axios
    .get(`https://api.openweathermap.org/data/2.5/weather?q=${selected.capital?.[0]}&appid=da42b720f23be71ec794601b6edb93b0&units=metric`
    )
    .then(response => {
      setWeather(response.data)
    })

}, [selected])

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
       // <p>Languages: {country.language}</p>
       // <p>Weather in  {country.weather}:</p>

      </div>
    )
  }

// (Show-nappi)

let countryDetail = null

  if (selected) {
  countryDetail = (
    <div>
      <h2>{selected.name.common}</h2>
      <p>capital: {selected.capital}</p>
      <p>area: {selected.area}</p>

      {weather && (
        <div>
          <h3>Weather in {selected.capital}</h3>
          <p>temperature: {weather.main.temp} °C</p>
          <p>wind: {weather.wind.speed} m/s</p>
          <img
            src={`https://openweathermap.org/img/wn/${weather.weather[0].icon}@2x.png`}
            alt="weather icon"
          />
        </div>
      )}
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