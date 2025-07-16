import React, { useState } from 'react';

const BookingForm = () => {
  const [formData, setFormData] = useState({
    name: '',
    date: '',
    time: '',
    guests: 1,
    occasion: 'Birthday'
  });

  const [errors, setErrors] = useState({});

  const times = ['17:00', '18:00', '19:00', '20:00', '21:00', '22:00'];

  const validate = () => {
    const newErrors = {};
    if (!formData.name) newErrors.name = 'Name is required';
    if (!formData.date) newErrors.date = 'Date is required';
    if (!formData.time) newErrors.time = 'Time is required';
    if (formData.guests < 1) newErrors.guests = 'At least one guest required';
    return newErrors;
  };

  const handleChange = e => {
    const { name, value } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: value
    }));
  };

  const handleSubmit = e => {
    e.preventDefault();
    const validationErrors = validate();
    if (Object.keys(validationErrors).length) {
      setErrors(validationErrors);
    } else {
      alert('Table booked successfully!');
      console.log(formData);
    }
  };

  return (
    <form onSubmit={handleSubmit} aria-label="Booking Form">
      <label htmlFor="name">Name:</label>
      <input
        id="name"
        name="name"
        type="text"
        aria-required="true"
        value={formData.name}
        onChange={handleChange}
      />
      {errors.name && <span role="alert">{errors.name}</span>}

      <label htmlFor="date">Date:</label>
      <input
        id="date"
        name="date"
        type="date"
        value={formData.date}
        onChange={handleChange}
      />
      {errors.date && <span role="alert">{errors.date}</span>}

      <label htmlFor="time">Time:</label>
      <select
        id="time"
        name="time"
        value={formData.time}
        onChange={handleChange}
      >
        <option value="">Select a time</option>
        {times.map(t => (
          <option key={t} value={t}>{t}</option>
        ))}
      </select>
      {errors.time && <span role="alert">{errors.time}</span>}

      <label htmlFor="guests">Number of Guests:</label>
      <input
        id="guests"
        name="guests"
        type="number"
        min="1"
        max="10"
        value={formData.guests}
        onChange={handleChange}
      />
      {errors.guests && <span role="alert">{errors.guests}</span>}

      <label htmlFor="occasion">Occasion:</label>
      <select
        id="occasion"
        name="occasion"
        value={formData.occasion}
        onChange={handleChange}
      >
        <option>Birthday</option>
        <option>Anniversary</option>
      </select>

      <button type="submit">Book Table</button>
    </form>
  );
};

export default BookingForm;
import React from 'react';
import BookingForm from './components/BookingForm';

function App() {
  return (
    <div>
      <h1>Little Lemon Table Reservation</h1>
      <BookingForm />
    </div>
  );
}

export default App;
import React from 'react';
import BookingForm from './components/BookingForm';

function App() {
  return (
    <div>
      <h1>Little Lemon Table Reservation</h1>
      <BookingForm />
    </div>
  );
}

export default App;
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
import { render, screen, fireEvent } from '@testing-library/react';
import BookingForm from '../components/BookingForm';

test('renders booking form with all fields', () => {
  render(<BookingForm />);
  expect(screen.getByLabelText(/name/i)).toBeInTheDocument();
  expect(screen.getByLabelText(/date/i)).toBeInTheDocument();
  expect(screen.getByLabelText(/time/i)).toBeInTheDocument();
  expect(screen.getByLabelText(/number of guests/i)).toBeInTheDocument();
});

test('form shows validation error when submitted empty', () => {
  render(<BookingForm />);
  fireEvent.click(screen.getByText(/book table/i));
  expect(screen.getByText(/name is required/i)).toBeInTheDocument();
});

