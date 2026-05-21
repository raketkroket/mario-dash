import React from 'react';
import DashboardLayout from '../components/DashboardLayout.vue';
import DataSourceBadge from '../components/DataSourceBadge.vue';
import { motion } from 'framer-motion';
import {
  LineChart,
  Line,
  XAxis,
  YAxis,
  CartesianGrid,
  Tooltip,
  ResponsiveContainer,
  Legend,
  PieChart,
  Pie,
  Cell,
} from 'recharts';

const groeiData = [
  { jaar: '2020', Frontend: 38, Backend: 44, API: 18 },
  { jaar: '2021', Frontend: 52, Backend: 61, API: 26 },
  { jaar: '2022', Frontend: 67, Backend: 79, API: 35 },
  { jaar: '2023', Frontend: 78, Backend: 91, API: 44 },
  { jaar: '2024', Frontend: 88, Backend: 104, API: 56 },
  { jaar: '2025', Frontend: 96, Backend: 115, API: 65 },
];

const typeVerdeling = [
  { name: 'Backend', value: 43, color: '#8b5cf6' },
  { name: 'Frontend', value: 36, color: '#0ea5e9' },
  { name: 'API', value: 14, color: '#10b981' },
  { name: 'Full-stack', value: 7, color: '#f59e0b' },
];

const trendItems = [
  { titel: 'AI-integratie in development', groei: '+67%', beschrijving: 'Vacatures die AI/ML kennis vereisen groeien explosief' },
  { titel: 'Remote-first posities', groei: '+44%', beschrijving: 'Meer werkgevers bieden hybride of volledig remote aan' },
  { titel: 'Cloud-native skills', groei: '+38%', beschrijving: 'AWS, Azure en GCP worden steeds vaker vereist' },
  { titel: 'TypeScript adoptie', groei: '+29%', beschrijving: 'TypeScript vervangt JavaScript in steeds meer vacatures' },
];

const Trends: React.FC = () => (
  <DashboardLayout title="Trends" subtitle="Markttrends en ontwikkelingen in de developer arbeidsmarkt">
    <div className="space-y-6 max-w-[1400px]">
      <DataSourceBadge source="UWV & CBS" lastUpdated="15 jan 2026, 08:00" status="live" />

      <div className="grid grid-cols-2 md:grid-cols-4 gap-4">
        {trendItems.map((item, i) => (
          <motion.div
            key={item.titel}
            initial={{ opacity: 0, y: 16 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ delay: i * 0.08 }}
            className="bg-white border border-slate-200 rounded-xl p-5 hover:shadow-md transition-all duration-300"
          >
            <p className="text-2xl font-bold font-heading text-sky-600 mb-1">{item.groei}</p>
            <p className="text-sm font-semibold text-slate-800 mb-1">{item.titel}</p>
            <p className="text-xs text-slate-500">{item.beschrijving}</p>
          </motion.div>
        ))}
      </div>

      <div className="grid grid-cols-1 xl:grid-cols-2 gap-6">
        <div className="bg-white border border-slate-200 rounded-xl p-6">
          <div className="mb-6">
            <h2 className="font-heading font-bold text-slate-800 text-base">Groei per developer type (2020–2025)</h2>
            <p className="text-xs text-slate-500 mt-0.5">Cumulatieve vacaturegroei per jaar</p>
          </div>
          <ResponsiveContainer width="100%" height={280}>
            <LineChart data={groeiData} margin={{ top: 4, right: 4, left: -20, bottom: 0 }}>
              <CartesianGrid strokeDasharray="3 3" stroke="#f1f5f9" />
              <XAxis dataKey="jaar" tick={{ fontSize: 11, fill: '#94a3b8' }} axisLine={false} tickLine={false} />
              <YAxis tick={{ fontSize: 11, fill: '#94a3b8' }} axisLine={false} tickLine={false} />
              <Tooltip contentStyle={{ background: '#fff', border: '1px solid #e2e8f0', borderRadius: '8px', fontSize: '12px' }} />
              <Legend wrapperStyle={{ fontSize: '12px', paddingTop: '16px' }} />
              <Line type="monotone" dataKey="Frontend" stroke="#0ea5e9" strokeWidth={2.5} dot={{ r: 4, fill: '#0ea5e9' }} />
              <Line type="monotone" dataKey="Backend" stroke="#8b5cf6" strokeWidth={2.5} dot={{ r: 4, fill: '#8b5cf6' }} />
              <Line type="monotone" dataKey="API" stroke="#10b981" strokeWidth={2.5} dot={{ r: 4, fill: '#10b981' }} />
            </LineChart>
          </ResponsiveContainer>
        </div>

        <div className="bg-white border border-slate-200 rounded-xl p-6">
          <div className="mb-6">
            <h2 className="font-heading font-bold text-slate-800 text-base">Verdeling vacatures per type</h2>
            <p className="text-xs text-slate-500 mt-0.5">Procentuele verdeling 2025</p>
          </div>
          <div className="flex items-center gap-6">
            <ResponsiveContainer width="60%" height={240}>
              <PieChart>
                <Pie data={typeVerdeling} cx="50%" cy="50%" innerRadius={60} outerRadius={90} paddingAngle={3} dataKey="value">
                  {typeVerdeling.map((entry, index) => (
                    <Cell key={`cell-${index}`} fill={entry.color} />
                  ))}
                </Pie>
                <Tooltip
                  formatter={(value: number) => [`${value}%`, 'Aandeel']}
                  contentStyle={{ background: '#fff', border: '1px solid #e2e8f0', borderRadius: '8px', fontSize: '12px' }}
                />
              </PieChart>
            </ResponsiveContainer>
            <div className="space-y-3 flex-1">
              {typeVerdeling.map((item) => (
                <div key={item.name} className="flex items-center gap-2">
                  <span className="w-3 h-3 rounded-full shrink-0" style={{ background: item.color }} />
                  <span className="text-sm text-slate-600 flex-1">{item.name}</span>
                  <span className="text-sm font-bold text-slate-800">{item.value}%</span>
                </div>
              ))}
            </div>
          </div>
        </div>
      </div>

      <div className="bg-white border border-slate-200 rounded-xl p-6">
        <h2 className="font-heading font-bold text-slate-800 text-base mb-4">Prognose 2026</h2>
        <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
          {[
            { label: 'Verwachte vacatures Q1 2026', value: '680+', trend: 'Stijgend' },
            { label: 'Verwacht salarisgroei', value: '+5–8%', trend: 'Stabiel' },
            { label: 'Tekort developers', value: '220+', trend: 'Groeiend' },
          ].map((item) => (
            <div key={item.label} className="bg-slate-50 rounded-lg p-4 border border-slate-100">
              <p className="text-xs text-slate-500 mb-1">{item.label}</p>
              <p className="text-xl font-bold font-heading text-slate-800">{item.value}</p>
              <p className="text-xs text-sky-600 font-medium mt-1">{item.trend}</p>
            </div>
          ))}
        </div>
      </div>
    </div>
  </DashboardLayout>
);

export default Trends;