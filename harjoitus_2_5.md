
import courses from "./Course/courses"

const App = () => {

return <div>
  <h1>Web development curriculum</h1>
    {courses.map(course => (
      <Course key={course.id} course={course} />
    ))}
  </div>
}

const Part = ({ part }) => {
  return (
    <li>
      {part.name} {part.exercises}
    </li>
  )
}

const Course = ({ course }) => {
  return (
    <div>
      <h2>{course.name}</h2>
      <ul>
        {course.parts.map(part => (
          <Part key={part.id} part={part} />
        ))}
      </ul>
      <h3>Total of {course.parts.reduce((sum, part) => sum + part.exercises, 0)} exercises</h3>
    </div>
  ) 
}

export default App

