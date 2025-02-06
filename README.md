import { useState } from 'react'
import { Button } from "/components/ui/button"
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from "/components/ui/card"
import { Input } from "/components/ui/input"
import { Label } from "/components/ui/label"
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select"
import { Calendar } from "lucide-react"
import { format, differenceInDays } from 'date-fns'

export default function HotelBooking() {
  const [checkInDate, setCheckInDate] = useState<Date | null>(null)
  const [checkOutDate, setCheckOutDate] = useState<Date | null>(null)
  const [roomType, setRoomType] = useState<string>('single')
  const [totalCost, setTotalCost] = useState<number>(0)

  const roomPrices = {
    single: 100,
    double: 150,
    suite: 250,
  }

  const calculateTotalCost = () => {
    if (checkInDate && checkOutDate) {
      const numberOfNights = differenceInDays(checkOutDate, checkInDate)
      const costPerNight = roomPrices[roomType]
      setTotalCost(numberOfNights * costPerNight)
    } else {
      setTotalCost(0)
    }
  }

  const handleCheckInDateChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const date = new Date(e.target.value)
    setCheckInDate(date)
    if (checkOutDate && date >= checkOutDate) {
      setCheckOutDate(null)
    }
    calculateTotalCost()
  }

  const handleCheckOutDateChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const date = new Date(e.target.value)
    if (checkInDate && date > checkInDate) {
      setCheckOutDate(date)
      calculateTotalCost()
    }
  }

  const handleRoomTypeChange = (value: string) => {
    setRoomType(value)
    calculateTotalCost()
  }

  return (
    <Card className="w-full max-w-md mx-auto mt-10">
      <CardHeader>
        <CardTitle className="text-2xl font-bold">Hotel Booking</CardTitle>
        <CardDescription>Select your check-in and check-out dates, and choose a room type.</CardDescription>
      </CardHeader>
      <CardContent className="space-y-4">
        <div>
          <Label htmlFor="check-in-date" className="block text-sm font-medium leading-6 text-gray-900">
            Check-in Date
          </Label>
          <div className="relative mt-2 rounded-md shadow-sm">
            <div className="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
              <Calendar className="h-5 w-5 text-gray-400" aria-hidden="true" />
            </div>
            <Input
              type="date"
              id="check-in-date"
              className="block w-full rounded-md border-0 py-1.5 pl-10 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
              onChange={handleCheckInDateChange}
            />
          </div>
        </div>
        <div>
          <Label htmlFor="check-out-date" className="block text-sm font-medium leading-6 text-gray-900">
            Check-out Date
          </Label>
          <div className="relative mt-2 rounded-md shadow-sm">
            <div className="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
              <Calendar className="h-5 w-5 text-gray-400" aria-hidden="true" />
            </div>
            <Input
              type="date"
              id="check-out-date"
              className="block w-full rounded-md border-0 py-1.5 pl-10 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
              onChange={handleCheckOutDateChange}
              min={checkInDate ? format(checkInDate, 'yyyy-MM-dd') : undefined}
            />
          </div>
        </div>
        <div>
          <Label htmlFor="room-type" className="block text-sm font-medium leading-6 text-gray-900">
            Room Type
          </Label>
          <Select onValueChange={handleRoomTypeChange} defaultValue={roomType}>
            <SelectTrigger className="w-full">
              <SelectValue placeholder="Select a room type" />
            </SelectTrigger>
            <SelectContent>
              <SelectItem value="single">Single Room - $100/night</SelectItem>
              <SelectItem value="double">Double Room - $150/night</SelectItem>
              <SelectItem value="suite">Suite - $250/night</SelectItem>
            </SelectContent>
          </Select>
        </div>
      </CardContent>
      <CardFooter className="flex justify-between">
        <div>
          <p className="text-sm font-medium leading-6 text-gray-900">Total Cost</p>
          <p className="text-2xl font-bold text-gray-900">${totalCost}</p>
        </div>
        <Button>Book Now</Button>
      </CardFooter>
    </Card>
  )
}
