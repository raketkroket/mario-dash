import React from 'react';
import {
  RadarChart,
  PolarGrid,
  PolarAngleAxis,
  Radar,
  ResponsiveContainer,
  Legend,
  Tooltip,
} from 'recharts';

const data = [
  { subject: 'Frontend', vraag: 96, aanbod: 72 },
  { subject: 'Backend', vraag: 115, aanbod: 68 },
  { subject: 'API', vraag: 65, aanbod: 45 },
  { subject: 'Full-stack', vraag: 88, aanbod: 54 },
  { subject: 'DevOps', vraag: 52, aanbod: 38 },
  { subject: 'Mobile', vraag: 41, aanbod: 29 },
];

const VraagAanbodChart: React.FC = () => (
  <div className="bg-white border border-slate-200 rounded-xl p-6">
    <div className="flex items-center justify-between mb-6">
      <div>
        <h2 className="font-heading font-bold text-slate-800 text-base">
          Vraag vs. Aanbod
        </h2>
        <p className="text-xs text-slate-500 mt-0.5">Balans per developer type</p>
      </div>
      <span className="text-xs text-slate-400 bg-slate-100 px-2 py-1 rounded-md font-mono">
        2025
      </span>
    </div>
    <ResponsiveContainer width="100%" height={260}>
      <RadarChart data={data}>
        <PolarGrid stroke="#e2e8f0" />
        <PolarAngleAxis dataKey="subject" tick={{ fontSize: 11, fill: '#64748b' }} />
        <Radar name="Vraag" dataKey="vraag" stroke="#0ea5e9" fill="#0ea5e9" fillOpacity={0.15} strokeWidth={2} />
        <Radar name="Aanbod" dataKey="aanbod" stroke="#8b5cf6" fill="#8b5cf6" fillOpacity={0.15} strokeWidth={2} />
        <Legend wrapperStyle={{ fontSize: '12px', paddingTop: '12px' }} />
        <Tooltip
          contentStyle={{
            background: '#fff',
            border: '1px solid #e2e8f0',
            borderRadius: '8px',
            fontSize: '12px',
          }}
        />
      </RadarChart>
    </ResponsiveContainer>
  </div>
);

export default VraagAanbodChart;