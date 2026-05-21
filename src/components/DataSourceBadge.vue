import React from 'react';
import { Database, Clock } from 'lucide-react';

interface DataSourceBadgeProps {
  source: string;
  lastUpdated: string;
  status?: 'live' | 'cached' | 'error';
}

const DataSourceBadge: React.FC<DataSourceBadgeProps> = ({
  source,
  lastUpdated,
  status = 'live',
}) => {
  const statusConfig = {
    live: { color: 'text-emerald-600', dot: 'bg-emerald-400', label: 'Live' },
    cached: { color: 'text-amber-600', dot: 'bg-amber-400', label: 'Gecached' },
    error: { color: 'text-rose-600', dot: 'bg-rose-400', label: 'Fout' },
  };
  const s = statusConfig[status];

  return (
    <div className="flex flex-wrap items-center gap-4 text-xs text-slate-500 bg-slate-50 border border-slate-200 rounded-lg px-4 py-2.5">
      <div className="flex items-center gap-1.5">
        <Database size={13} className="text-slate-400" />
        <span>Bron: <span className="font-semibold text-slate-700">{source}</span></span>
      </div>
      <div className="w-px h-3 bg-slate-200 hidden sm:block" />
      <div className="flex items-center gap-1.5">
        <Clock size={13} className="text-slate-400" />
        <span>Update: <span className="font-semibold text-slate-700">{lastUpdated}</span></span>
      </div>
      <div className="w-px h-3 bg-slate-200 hidden sm:block" />
      <div className="flex items-center gap-1.5">
        <span className={`w-1.5 h-1.5 rounded-full ${s.dot} ${status === 'live' ? 'animate-pulse' : ''}`} />
        <span className={`font-semibold ${s.color}`}>{s.label}</span>
      </div>
    </div>
  );
};

export default DataSourceBadge;