import React from 'react';
import {
  BarChart,
  Bar,
  XAxis,
  YAxis,
  CartesianGrid,
  Tooltip,
  ResponsiveContainer,
  Legend,
} from 'recharts';

const data = [
  { niveau: 'Junior', Frontend: 38000, Backend: 40000, API: 39000 },
  { niveau: 'Medior', Frontend: 54000, Backend: 58000, API: 56000 },
  { niveau: 'Senior', Frontend: 72000, Backend: 78000, API: 75000 },
  { niveau: 'Lead', Frontend: 88000, Backend: 95000, API: 91000 },
];

const formatSalaris = (value: number) => `€${(value / 1000).toFixed(0)}k`;

const SalarisChart: React.FC = () => (
  <div className="bg-white border border-slate-200 rounded-xl p-6">
    <div className="flex items-center justify-between mb-6">
      <div>
        <h2 className="font-heading font-bold text-slate-800 text-base">
          Gemiddeld salaris per niveau
        </h2>
        <p className="text-xs text-slate-500 mt-0.5">Bruto jaarsalaris in euro's</p>
      </div>
      <span className="text-xs text-slate-400 bg-slate-100 px-2 py-1 rounded-md font-mono">
        Bron: CBS
      </span>
    </div>
    <ResponsiveContainer width="100%" height={260}>
      <BarChart data={data} margin={{ top: 4, right: 4, left: -8, bottom: 0 }}>
        <CartesianGrid strokeDasharray="3 3" stroke="#f1f5f9" />
        <XAxis dataKey="niveau" tick={{ fontSize: 11, fill: '#94a3b8' }} axisLine={false} tickLine={false} />
        <YAxis tickFormatter={formatSalaris} tick={{ fontSize: 11, fill: '#94a3b8' }} axisLine={false} tickLine={false} />
        <Tooltip
          formatter={(value: number) => [`€${value.toLocaleString('nl-NL')}`, '']}
          contentStyle={{
            background: '#fff',
            border: '1px solid #e2e8f0',
            borderRadius: '8px',
            fontSize: '12px',
          }}
        />
        <Legend wrapperStyle={{ fontSize: '12px', paddingTop: '16px' }} />
        <Bar dataKey="Frontend" fill="#0ea5e9" radius={[4, 4, 0, 0]} maxBarSize={24} />
        <Bar dataKey="Backend" fill="#8b5cf6" radius={[4, 4, 0, 0]} maxBarSize={24} />
        <Bar dataKey="API" fill="#10b981" radius={[4, 4, 0, 0]} maxBarSize={24} />
      </BarChart>
    </ResponsiveContainer>
  </div>
);

export default SalarisChart;