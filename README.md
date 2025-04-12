import { Controller, Get, Post, Body } from '@nestjs/common';
import { BookingsService } from './bookings.service';

@Controller('bookings')
export class BookingsController {
  constructor(private readonly bookingsService: BookingsService) {}

  @Get()
  getAll() {
    return this.bookingsService.findAll();
  }

  @Post()
  create(@Body() bookingDto: any) {
    return this.bookingsService.create(bookingDto);
  }
}

import { Injectable } from '@nestjs/common';

@Injectable()
export class BookingsService {
  private bookings = [];

  findAll() {
    return this.bookings;
  }

  create(booking: any) {
    this.bookings.push(booking);
    return { message: 'Booking successful!', booking };
  }
}

import Link from 'next/link';

export default function Home() {
  return (
    <main className="p-8">
      <h1 className="text-3xl font-bold mb-4">Welcome to Hotel Booking</h1>
      <Link href="/hotels" className="text-blue-500 underline">
        Browse Hotels
      </Link>
    </main>
  );
}

import Link from 'next/link';

const hotels = [
  { id: 1, name: 'Hotel Paradise', location: 'Goa' },
  { id: 2, name: 'Ocean View', location: 'Kerala' },
];

export default function Hotels() {
  return (
    <div className="p-8">
      <h2 className="text-2xl mb-4">Available Hotels</h2>
      <ul>
        {hotels.map(hotel => (
          <li key={hotel.id} className="mb-2">
            {hotel.name} - {hotel.location}  
            <Link href={`/booking?id=${hotel.id}`} className="ml-4 text-green-600">Book</Link>
          </li>
        ))}
      </ul>
    </div>
  );
}
import { useRouter } from 'next/router';
import { useState } from 'react';

export default function Booking() {
  const router = useRouter();
  const { id } = router.query;

  const [name, setName] = useState('');

  const handleBooking = async () => {
    await fetch('http://localhost:3001/bookings', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ hotelId: id, name }),
    });
    alert('Booking confirmed!');
  };

  return (
    <div className="p-8">
      <h2 className="text-2xl mb-4">Book Hotel #{id}</h2>
      <input
        className="border p-2 mr-4"
        placeholder="Your name"
        value={name}
        onChange={(e) => setName(e.target.value)}
      />
      <button className="bg-blue-600 text-white px-4 py-2" onClick={handleBooking}>
        Confirm Booking
      </button>
    </div>
  );
}
{
  "info": { "name": "Hotel Booking API", "_postman_id": "...", "schema": "https://schema.getpostman.com" },
  "item": [
    {
      "name": "Get All Bookings",
      "request": {
        "method": "GET",
        "url": "http://localhost:3001/bookings"
      }
    },
    {
      "name": "Create Booking",
      "request": {
        "method": "POST",
        "header": [{ "key": "Content-Type", "value": "application/json" }],
        "url": "http://localhost:3001/bookings",
        "body": {
          "mode": "raw",
          "raw": "{ \"hotelId\": 1, \"name\": \"John Doe\" }"
        }
      }
    }
  ]
}
