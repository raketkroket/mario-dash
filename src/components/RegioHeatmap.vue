import React from 'react';
import { motion } from 'framer-motion';
import { MapPin } from 'lucide-react';

const steden = [
  { naam: 'Almere', vacatures: 312, groei: 18, lat: 52.37, lng: 5.22, size: 'xl' },
  { naam: 'Lelystad', vacatures: 124, groei: 12, lat: 52.51, lng: 5.47, size: 'lg' },
  { naam: 'Emmeloord', vacatures: 67, groei: 8, lat: 52.71, lng: 5.75, size: 'md' },
  { naam: 'Dronten', vacatures: 45, groei: 5, lat: 52.52, lng: 5.72, size: 'sm' },
  { naam: 'Zeewolde', vacatures: 38, groei: 14, lat: 52.33, lng: 5.54, size: 'sm' },
  { naam: 'Urk', vacatures: 22, groei: 3, lat: 52.66, lng: 5.60, size: 'xs' },
];

const sizeMap: Record<string, string> = {
  xl: 'w-16 h-16 text-base',
  lg: 'w-12 h-12 text-sm',
  md: 'w-10 h-10 text-xs',
  sm: 'w-8 h-8 text-xs',
  xs: 'w-7 h-7 text-xs',
};

const RegioHeatmap: React.FC = () => (
  <div className="bg-white border border-slate-200 rounded-xl p-6">
    <div className="flex items-center justify-between mb-6">
      <div>
        <h2 className="font-heading font-bold text-slate-800 text-base">
          Werkgelegenheid per stad
        </h2>
        <p className="text-xs text-slate-500 mt-0.5">Openstaande vacatures in Flevoland</p>
      </div>
      <span className="text-xs text-slate-400 bg-slate-100 px-2 py-1 rounded-md font-mono">
        Kaart
      </span>
    </div>

    <div className="relative bg-gradient-to-br from-sky-50 to-slate-100 rounded-lg h-52 overflow-hidden border border-slate-100 mb-4">
      <div className="absolute inset-0 opacity-10">
        {[...Array(8)].map((_, i) => (
          <div key={`v-${i}`} className="absolute border border-sky-300" style={{ left: `${i * 14}%`, top: 0, bottom: 0, width: '1px' }} />
        ))}
        {[...Array(6)].map((_, i) => (
          <div key={`h-${i}`} className="absolute border border-sky-300" style={{ top: `${i * 18}%`, left: 0, right: 0, height: '1px' }} />
        ))}
      </div>
      <div className="absolute top-2 left-3 text-[10px] text-slate-400 font-mono">Flevoland</div>
      {steden.map((stad, i) => {
        const x = ((stad.lng - 5.1) / 0.8) * 100;
        const y = ((52.75 - stad.lat) / 0.5) * 100;
        return (
          <motion.div
            key={stad.naam}
            initial={{ scale: 0, opacity: 0 }}
            animate={{ scale: 1, opacity: 1 }}
            transition={{ delay: i * 0.1, type: 'spring', stiffness: 200 }}
            className="absolute -translate-x-1/2 -translate-y-1/2 group cursor-pointer"
            style={{ left: `${Math.min(Math.max(x, 8), 92)}%`, top: `${Math.min(Math.max(y, 8), 88)}%` }}
          >
            <div className={`${sizeMap[stad.size] ?? sizeMap['sm']} rounded-full bg-sky-500/20 border-2 border-sky-500 flex items-center justify-center font-bold text-sky-700 hover:bg-sky-500/30 transition-all duration-200`}>
              {stad.vacatures > 99 ? '99+' : stad.vacatures}
            </div>
            <div className="absolute bottom-full left-1/2 -translate-x-1/2 mb-1 bg-slate-800 text-white text-[10px] px-2 py-1 rounded whitespace-nowrap opacity-0 group-hover:opacity-100 transition-opacity duration-200 pointer-events-none z-10">
              {stad.naam}: {stad.vacatures} vacatures
            </div>
          </motion.div>
        );
      })}
    </div>

    <div className="space-y-2">
      {steden.map((stad) => (
        <div key={stad.naam} className="flex items-center gap-3">
          <MapPin size={12} className="text-sky-500 shrink-0" />
          <span className="text-xs text-slate-600 w-24 shrink-0">{stad.naam}</span>
          <div className="flex-1 bg-slate-100 rounded-full h-1.5 overflow-hidden">
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
);

export default RegioHeatmap;