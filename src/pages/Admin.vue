import React, { useState } from 'react';
import { motion } from 'framer-motion';
import {
  Database,
  RefreshCw,
  CheckCircle,
  AlertCircle,
  Settings,
  Plus,
  Trash2,
  Eye,
  EyeOff,
} from 'lucide-react';
import DashboardLayout from '../components/DashboardLayout.vue';
import { toast } from 'react-toastify';

const apiSources = [
  { id: 1, naam: 'UWV Open Data API', url: 'https://api.uwv.nl/v1/vacatures', status: 'actief', lastSync: '15 jan 2026, 08:00', interval: '6 uur' },
  { id: 2, naam: 'CBS StatLine API', url: 'https://opendata.cbs.nl/ODataApi/odata', status: 'actief', lastSync: '15 jan 2026, 06:00', interval: '24 uur' },
  { id: 3, naam: 'Arbeidsmarktdata.nl', url: 'https://api.arbeidsmarktdata.nl/v2', status: 'inactief', lastSync: '10 jan 2026, 12:00', interval: 'Handmatig' },
];

const Admin: React.FC = () => {
  const [showKey, setShowKey] = useState(false);
  const [syncing, setSyncing] = useState<number | null>(null);

  const handleSync = (id: number, naam: string) => {
    setSyncing(id);
    setTimeout(() => {
      setSyncing(null);
      toast.success(`${naam} succesvol gesynchroniseerd`);
    }, 2000);
  };

  const handleSave = () => {
    toast.success('Instellingen opgeslagen');
  };

  return (
    <DashboardLayout title="Admin Panel" subtitle="Beheer van datasets, API-instellingen en configuratie">
      <div className="space-y-6 max-w-4xl">
        <div className="bg-amber-50 border border-amber-200 rounded-xl p-4 flex items-start gap-3">
          <AlertCircle size={18} className="text-amber-600 shrink-0 mt-0.5" />
          <div>
            <p className="text-sm font-semibold text-amber-800">Beheerdersmodus actief</p>
            <p className="text-xs text-amber-700 mt-0.5">
              Wijzigingen in dit paneel beïnvloeden de live data van het dashboard. Ga zorgvuldig te werk.
            </p>
          </div>
        </div>

        <section>
          <h2 className="font-heading font-bold text-slate-800 text-base mb-4 flex items-center gap-2">
            <Database size={18} className="text-sky-600" />
            API Databronnen
          </h2>
          <div className="space-y-3">
            {apiSources.map((src, i) => (
              <motion.div
                key={src.id}
                initial={{ opacity: 0, y: 10 }}
                animate={{ opacity: 1, y: 0 }}
                transition={{ delay: i * 0.08 }}
                className="bg-white border border-slate-200 rounded-xl p-5"
              >
                <div className="flex items-start justify-between gap-4">
                  <div className="flex-1 min-w-0">
                    <div className="flex items-center gap-2 mb-1">
                      <h3 className="text-sm font-semibold text-slate-800">{src.naam}</h3>
                      <span
                        className={`text-[10px] px-2 py-0.5 rounded-full font-semibold ${
                          src.status === 'actief'
                            ? 'bg-emerald-100 text-emerald-700'
                            : 'bg-slate-100 text-slate-500'
                        }`}
                      >
                        {src.status}
                      </span>
                    </div>
                    <p className="text-xs text-slate-400 font-mono truncate">{src.url}</p>
                    <div className="flex items-center gap-4 mt-2 text-xs text-slate-500">
                      <span>Laatste sync: <span className="font-medium text-slate-700">{src.lastSync}</span></span>
                      <span>Interval: <span className="font-medium text-slate-700">{src.interval}</span></span>
                    </div>
                  </div>
                  <div className="flex items-center gap-2 shrink-0">
                    <button
                      onClick={() => handleSync(src.id, src.naam)}
                      disabled={syncing === src.id}
                      className="flex items-center gap-1.5 px-3 py-1.5 text-xs font-medium bg-sky-50 text-sky-700 border border-sky-200 rounded-lg hover:bg-sky-100 transition-all duration-200 disabled:opacity-50 focus:outline-none focus:ring-2 focus:ring-sky-500/40"
                    >
                      <RefreshCw size={12} className={syncing === src.id ? 'animate-spin' : ''} />
                      {syncing === src.id ? 'Syncing...' : 'Sync'}
                    </button>
                    <button
                      aria-label="Verwijder databron"
                      className="p-1.5 text-slate-400 hover:text-rose-500 hover:bg-rose-50 rounded-lg transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-rose-500/40"
                    >
                      <Trash2 size={14} />
                    </button>
                  </div>
                </div>
              </motion.div>
            ))}
          </div>
          <button className="mt-3 flex items-center gap-2 px-4 py-2 text-sm font-medium text-sky-700 border border-sky-200 bg-sky-50 rounded-lg hover:bg-sky-100 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-sky-500/40">
            <Plus size={14} />
            Databron toevoegen
          </button>
        </section>

        <section>
          <h2 className="font-heading font-bold text-slate-800 text-base mb-4 flex items-center gap-2">
            <Settings size={18} className="text-sky-600" />
            API Configuratie
          </h2>
          <div className="bg-white border border-slate-200 rounded-xl p-6 space-y-4">
            <div>
              <label className="block text-xs font-semibold text-slate-600 mb-1.5">UWV API Sleutel</label>
              <div className="flex gap-2">
                <div className="relative flex-1">
                  <input
                    type={showKey ? 'text' : 'password'}
                    defaultValue="uwv_live_sk_flevoland_2025_xxxx"
                    className="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 font-mono focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200"
                  />
                </div>
                <button
                  onClick={() => setShowKey(!showKey)}
                  aria-label={showKey ? 'Verberg sleutel' : 'Toon sleutel'}
                  className="p-2 border border-slate-200 rounded-lg text-slate-500 hover:text-sky-600 hover:bg-sky-50 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-sky-500/40"
                >
                  {showKey ? <EyeOff size={16} /> : <Eye size={16} />}
                </button>
              </div>
            </div>

            <div>
              <label className="block text-xs font-semibold text-slate-600 mb-1.5">CBS API Endpoint</label>
              <input
                type="url"
                defaultValue="https://opendata.cbs.nl/ODataApi/odata/85066NED"
                className="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 font-mono focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200"
              />
            </div>

            <div className="grid grid-cols-2 gap-4">
              <div>
                <label className="block text-xs font-semibold text-slate-600 mb-1.5">Sync interval</label>
                <select className="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 bg-white focus:outline-none focus:ring-2 focus:ring-sky-500/40 text-slate-700">
                  <option>Elke 6 uur</option>
                  <option>Elke 12 uur</option>
                  <option>Dagelijks</option>
                  <option>Wekelijks</option>
                </select>
              </div>
              <div>
                <label className="block text-xs font-semibold text-slate-600 mb-1.5">Regio filter</label>
                <select className="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 bg-white focus:outline-none focus:ring-2 focus:ring-sky-500/40 text-slate-700">
                  <option>Flevoland (GM0034)</option>
                  <option>Almere (GM0034)</option>
                  <option>Lelystad (GM0995)</option>
                </select>
              </div>
            </div>

            <div className="flex items-center justify-between pt-2 border-t border-slate-100">
              <div className="flex items-center gap-2 text-xs text-emerald-600">
                <CheckCircle size={14} />
                <span>Verbinding actief — alle API's bereikbaar</span>
              </div>
              <button
                onClick={handleSave}
                className="px-4 py-2 text-sm font-semibold bg-sky-600 text-white rounded-lg hover:bg-sky-700 hover:scale-105 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-sky-500/40"
              >
                Opslaan
              </button>
            </div>
          </div>
        </section>

        <section>
          <h2 className="font-heading font-bold text-slate-800 text-base mb-4">Database status</h2>
          <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
            {[
              { label: 'Totale records', value: '48.291', icon: Database },
              { label: 'Laatste import', value: '15 jan 2026', icon: RefreshCw },
              { label: 'Opslaggebruik', value: '2.4 GB / 10 GB', icon: Settings },
            ].map((item) => (
              <div key={item.label} className="bg-white border border-slate-200 rounded-xl p-5">
                <div className="flex items-center gap-2 mb-2">
                  <item.icon size={16} className="text-sky-600" />
                  <p className="text-xs text-slate-500">{item.label}</p>
                </div>
                <p className="text-lg font-bold font-heading text-slate-800">{item.value}</p>
              </div>
            ))}
          </div>
        </section>
      </div>
    </DashboardLayout>
  );
};

export default Admin;