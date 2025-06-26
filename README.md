import { useState } from "react";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { Calendar, User, DollarSign } from "lucide-react";

export default function AngkasEarningsCalculator() {
  const [passengers, setPassengers] = useState([{ fare: "", tip: "" }]);
  const [totals, setTotals] = useState({
    daily: 0,
    weekly: 0,
    monthly: 0,
    yearly: 0,
    tipTotal: 0,
    passengerCount: 1,
    netDaily: 0,
  });

  const handleInputChange = (index, field, value) => {
    const updated = [...passengers];
    updated[index][field] = value;
    setPassengers(updated);
  };

  const addPassenger = () => {
    setPassengers([...passengers, { fare: "", tip: "" }]);
  };

  const reset = () => {
    setPassengers([{ fare: "", tip: "" }]);
    setTotals({
      daily: 0,
      weekly: 0,
      monthly: 0,
      yearly: 0,
      tipTotal: 0,
      passengerCount: 1,
      netDaily: 0,
    });
  };

  const calculate = () => {
    let grossDaily = 0;
    let tips = 0;
    passengers.forEach(p => {
      const fare = parseFloat(p.fare) || 0;
      const tip = parseFloat(p.tip) || 0;
      grossDaily += fare + tip;
      tips += tip;
    });
    const angkasCut = grossDaily * 0.2;
    const netDaily = grossDaily - angkasCut;

    setTotals({
      daily: grossDaily,
      weekly: netDaily * 7,
      monthly: netDaily * 30,
      yearly: netDaily * 365,
      tipTotal: tips,
      passengerCount: passengers.length,
      netDaily,
    });
  };

  return (
    <div className="p-4 max-w-xl mx-auto bg-[#f5f8ff] min-h-screen font-sans text-gray-800">
      <h1 className="text-2xl font-bold mb-4 text-blue-600 text-center">Angkas Earnings Calculator</h1>

      {passengers.map((p, index) => (
        <Card key={index} className="mb-2 bg-white shadow-sm">
          <CardContent className="flex gap-2 p-4 items-center">
            <User className="text-blue-600" />
            <input
              type="number"
              placeholder="Fare"
              value={p.fare}
              onChange={e => handleInputChange(index, "fare", e.target.value)}
              className="w-1/3 p-2 rounded border border-blue-200 focus:outline-none"
            />
            <span className="text-blue-600 font-bold">₱</span>
            <input
              type="number"
              placeholder="Tip"
              value={p.tip}
              onChange={e => handleInputChange(index, "tip", e.target.value)}
              className="w-1/3 p-2 rounded border border-blue-200 focus:outline-none"
            />
          </CardContent>
        </Card>
      ))}

      <div className="flex gap-2 mb-4 justify-center">
        <Button onClick={addPassenger} className="bg-orange-400 text-white hover:bg-orange-500">Add Passenger</Button>
        <Button onClick={reset} className="bg-gray-200 text-blue-900 hover:bg-gray-300">Reset</Button>
        <Button onClick={calculate} className="bg-green-500 text-white hover:bg-green-600">Calculate</Button>
      </div>

      <Card className="bg-white shadow-md">
        <CardContent className="p-4 space-y-2 text-lg">
          <div className="flex items-center gap-2"><Calendar className="text-blue-600" /> Gross Daily Total: <span className="font-semibold text-blue-700">₱{totals.daily.toFixed(2)}</span></div>
          <div className="flex items-center gap-2"><DollarSign className="text-sky-500" /> Net Daily Earnings (after 20%): <span className="font-semibold text-green-600">₱{totals.netDaily.toFixed(2)}</span></div>
          <div className="flex items-center gap-2"><Calendar className="text-blue-600" /> Weekly (Net): <span className="font-semibold">₱{totals.weekly.toFixed(2)}</span></div>
          <div className="flex items-center gap-2"><Calendar className="text-blue-600" /> Monthly (Net): <span className="font-semibold">₱{totals.monthly.toFixed(2)}</span></div>
          <div className="flex items-center gap-2"><Calendar className="text-blue-600" /> Yearly (Net): <span className="font-semibold">₱{totals.yearly.toFixed(2)}</span></div>
          <div className="flex items-center gap-2"><DollarSign className="text-blue-600" /> Total Tips: <span className="font-semibold">₱{totals.tipTotal.toFixed(2)}</span></div>
          <div className="flex items-center gap-2"><User className="text-blue-600" /> Passengers: <span className="font-semibold">{totals.passengerCount}</span></div>
        </CardContent>
      </Card>
    </div>
  );
}
