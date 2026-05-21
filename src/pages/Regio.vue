import React, { useState } from 'react';
import { motion } from 'framer-motion';
import { MapPin, TrendingUp, Briefcase, Users } from 'lucide-react';
import DashboardLayout from '../components/DashboardLayout.vue';
import DataSourceBadge from '../components/DataSourceBadge.vue';
import {
  BarChart,
  Bar,
  XAxis,
  YAxis,
  CartesianGrid,
  Tooltip,
  ResponsiveContainer,
  RadialBarChart,
  RadialBar,
  Legend,
} from 'recharts';

const steden = [
  { naam: 'Almere', vacatures: 312, werkzoekenden: 198, groei: 18, inwoners: 226000, techBedrijven: 84 },
  { naam: 'Lelystad', vacatures: 124, werkzoekenden: 89, groei: 12, inwoners: 78000, techBedrijven: 31 },
  { naam: 'Emmeloord', vacatures: 67, werkzoekenden: 52, groei: 8, inwoners: 28000, techBedrijven: 14 },
  { naam: 'Dronten', vacatures: 45, werkzoekenden: 38, groei: 5, inwoners: 42000, techBedrijven: 9 },
  { naam: 'Zeewolde', vacatures: 38, werkzoekenden: 22, groei: 14, inwoners: 22000, techBedrijven: 7 },
  { naam: 'Urk', vacatures: 22, werkzoekenden: 13, groei: 3, inwoners: 21000, techBedrijven: 4 },
];

const radialData = steden.map((s, i) => ({
  name: s.naam,
  vacatures: s.vacatures,
  fill: ['#0ea5e9', '#8b5cf6', '#10b981', '#f59e0b', '#ef4444', '#06b6d4'][i],
}));

const Regio: React.FC = () => {
  const [selected, setSelected] = useState<string | null>(null);
  const activeStad = steden.find((s) => s.naam === selected) ?? steden[0];

  return (
    <DashboardLayout title="Regio's" subtitle="Werkgelegenheid per regio in Flevoland">
      <div className="space-y-6 max-w-[1400px]">
        <DataSourceBadge source="CBS & UWV" lastUpdated="15 jan 2026, 08:00" status="live" />

        <div className="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-6 gap-3">
          {steden.map((stad, i) => (
            <motion.button
              key={stad.naam}
              initial={{ opacity: 0, y: 12 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{ delay: i * 0.07 }}
              onClick={() => setSelected(stad.naam === selected ? null : stad.naam)}
              className={`p-4 rounded-xl border text-left transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-sky-500/40 ${
                selected === stad.naam
                  ? 'border-sky-400 bg-sky-50 shadow-md'
                  : 'border-slate-200 bg-white hover:shadow-sm hover:-translate-y-0.5'
              }`}
            >
              <MapPin size={14} className={selected === stad.naam ? 'text-sky-600' : 'text-slate-400'} />
              <p className="text-sm font-bold text-slate-800 mt-2">{stad.naam}</p>
              <p className="text-xl font-heading font-bold text-sky-600">{stad.vacatures}</p>
              <p className="text-[10px] text-slate-500">vacatures</p>
              <p className="text-[10px] text-emerald-600 font-semibold mt-1">+{stad.groei}%</p>
            </motion.button>
          ))}
        </div>

        <div className="grid grid-cols-1 xl:grid-cols-2 gap-6">
          <div className="bg-white border border-slate-200 rounded-xl p-6">
            <h2 className="font-heading font-bold text-slate-800 text-base mb-1">Vacatures per stad</h2>
            <p className="text-xs text-slate-500 mb-6">Vergelijking openstaande vacatures</p>
            <ResponsiveContainer width="100%" height={280}>
              <BarChart data={steden} margin={{ top: 4, right: 4, left: -20, bottom: 0 }}>
                <CartesianGrid strokeDasharray="3 3" stroke="#f1f5f9" />
                <XAxis dataKey="naam" tick={{ fontSize: 11, fill: '#94a3b8' }} axisLine={false} tickLine={false} />
                <YAxis tick={{ fontSize: 11, fill: '#94a3b8' }} axisLine={false} tickLine={false} />
                <Tooltip
                  contentStyle={{ background: '#fff', border: '1px solid #e2e8f0', borderRadius: '8px', fontSize: '12px' }}
                />
                <Bar dataKey="vacatures" fill="#0ea5e9" radius={[4, 4, 0, 0]} maxBarSize={40} name="Vacatures" />
                <Bar dataKey="werkzoekenden" fill="#8b5cf6" radius={[4, 4, 0, 0]} maxBarSize={40} name="Werkzoekenden" />
              </BarChart>
            </ResponsiveContainer>
          </div>

          <div className="bg-white border border-slate-200 rounded-xl p-6">
            <h2 className="font-heading font-bold text-slate-800 text-base mb-1">Aandeel per stad</h2>
            <p className="text-xs text-slate-500 mb-4">Relatief vacatureaandeel in Flevoland</p>
            <ResponsiveContainer width="100%" height={280}>
              <RadialBarChart
                cx="50%"
                cy="50%"
                innerRadius="20%"
                outerRadius="90%"
                data={radialData}
                startAngle={180}
                endAngle={0}
              >
                <RadialBar dataKey="vacatures" background={{ fill: '#f8fafc' }} cornerRadius={4} />
                <Legend
                  iconSize={10}
                  wrapperStyle={{ fontSize: '11px', paddingTop: '8px' }}
                />
                <Tooltip
                  contentStyle={{ background: '#fff', border: '1px solid #e2e8f0', borderRadius: '8px', fontSize: '12px' }}
                />
              </RadialBarChart>
            </ResponsiveContainer>
          </div>
        </div>

        <div className="bg-white border border-slate-200 rounded-xl p-6">
          <h2 className="font-heading font-bold text-slate-800 text-base mb-4">
            Detailoverzicht — {activeStad.naam}
          </h2>
          <div className="grid grid-cols-2 md:grid-cols-4 gap-4 mb-6">
            {[
              { label: 'Openstaande vacatures', value: activeStad.vacatures, icon: Briefcase, color: 'text-sky-600 bg-sky-50' },
              { label: 'Werkzoekenden', value: activeStad.werkzoekenden, icon: Users, color: 'text-violet-600 bg-violet-50' },
              { label: 'Vacaturegroei', value: `+${activeStad.groei}%`, icon: TrendingUp, color: 'text-emerald-600 bg-emerald-50' },
              { label: 'Tech bedrijven', value: activeStad.techBedrijven, icon: MapPin, color: 'text-amber-600 bg-amber-50' },
            ].map((item) => (
              <div key={item.label} className="bg-slate-50 border border-slate-100 rounded-lg p-4">
                <div className={`w-8 h-8 rounded-lg ${item.color} flex items-center justify-center mb-3`}>
                  <item.icon size={16} />
                </div>
                <p className="text-xl font-bold font-heading text-slate-800">{item.value}</p>
                <p className="text-xs text-slate-500 mt-0.5">{item.label}</p>
              </div>
            ))}
          </div>
          <div className="space-y-2">
            {steden.map((stad) => (
              <div key={stad.naam} className="flex items-center gap-3">
                <span className="text-xs text-slate-600 w-24 shrink-0">{stad.naam}</span>
                <div className="flex-1 bg-slate-100 rounded-full h-2 overflow-hidden">
                  <div
                    className="h-full bg-sky-500 rounded-full transition-all duration-700"
                    style={{ width: `${(stad.vacatures / 312) * 100}%` }}
                  />
                </div>
                <span className="text-xs font-semibold text-slate-700 w-8 text-right">{stad.vacatures}</span>
                <span className="text-[10px] text-emerald-600 font-medium w-10 text-right">+{stad.groei}%</span>
              </div>
            ))}
          </div>
        </div>
      </div>
    </DashboardLayout>
  );
};

export default Regio;