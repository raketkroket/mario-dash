import React from 'react';
import { Link } from 'react-router-dom';
import { Home, AlertTriangle } from 'lucide-react';

const NotFound: React.FC = () => (
  <div className="min-h-screen bg-[hsl(220,20%,97%)] flex items-center justify-center px-6">
    <div className="text-center max-w-md">
      <div className="w-16 h-16 rounded-2xl bg-rose-50 border border-rose-100 flex items-center justify-center mx-auto mb-6">
        <AlertTriangle size={28} className="text-rose-500" />
      </div>
      <h1 className="font-heading font-bold text-slate-800 text-3xl mb-3">404</h1>
      <p className="text-slate-500 mb-6">Deze pagina bestaat niet of is verplaatst.</p>
      <Link
        to="/"
        className="inline-flex items-center gap-2 px-6 py-3 bg-sky-600 text-white text-sm font-semibold rounded-lg hover:bg-sky-700 hover:scale-105 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-sky-500/40"
      >
        <Home size={16} />
        Terug naar dashboard
      </Link>
    </div>
  </div>
);

export default NotFound;