import React, { useEffect } from 'react';
import { motion, AnimatePresence } from 'framer-motion';
import { X, MapPin, Users, Briefcase, TrendingUp, Calendar, Globe, Mail } from 'lucide-react';
import { toast } from 'react-toastify';

export interface Bedrijf {
  id: number;
  naam: string;
  type: string;
  stad: string;
  vacatures: number;
  medewerkers: number;
  groei: number;
  techStack: string[];
  opgericht: number;
}

interface BedrijfDetailModalProps {
  bedrijf: Bedrijf | null;
  onClose: () => void;
}

const BedrijfDetailModal: React.FC<BedrijfDetailModalProps> = ({ bedrijf, onClose }) => {
  useEffect(() => {
    const handler = (e: KeyboardEvent) => {
      if (e.key === 'Escape') onClose();
    };
    window.addEventListener('keydown', handler);
    return () => window.removeEventListener('keydown', handler);
  }, [onClose]);

  if (!bedrijf) return null;

  const handleFollow = () => {
    toast.success(`Je volgt nu ${bedrijf.naam}`);
  };

  const handleContact = () => {
    toast.success(`Contactverzoek verstuurd naar ${bedrijf.naam}`);
    onClose();
  };

  return (
    <AnimatePresence>
      <motion.div
        initial={{ opacity: 0 }}
        animate={{ opacity: 1 }}
        exit={{ opacity: 0 }}
        onClick={onClose}
        className="fixed inset-0 bg-slate-900/50 backdrop-blur-sm z-50 flex items-center justify-center p-4"
      >
        <motion.div
          initial={{ opacity: 0, scale: 0.95, y: 20 }}
          animate={{ opacity: 1, scale: 1, y: 0 }}
          exit={{ opacity: 0, scale: 0.95, y: 20 }}
          transition={{ duration: 0.2 }}
          onClick={(e) => e.stopPropagation()}
          className="bg-white rounded-2xl shadow-2xl max-w-2xl w-full max-h-[90vh] overflow-auto"
        >
          <div className="sticky top-0 bg-white border-b border-slate-200 px-6 py-4 flex items-start justify-between gap-4">
            <div className="flex items-start gap-3 flex-1 min-w-0">
              <div className="w-14 h-14 rounded-xl bg-gradient-to-br from-violet-100 to-violet-200 flex items-center justify-center text-xl font-bold text-violet-700 shrink-0">
                {bedrijf.naam.charAt(0)}
              </div>
              <div className="flex-1 min-w-0">
                <h2 className="font-heading font-bold text-slate-800 text-lg leading-tight">{bedrijf.naam}</h2>
                <p className="text-sm text-slate-500 mt-0.5">{bedrijf.type} · {bedrijf.stad}</p>
              </div>
            </div>
            <button
              onClick={onClose}
              aria-label="Sluit"
              className="p-2 rounded-lg text-slate-400 hover:text-slate-700 hover:bg-slate-100 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-sky-500/40"
            >
              <X size={18} />
            </button>
          </div>

          <div className="px-6 py-5 space-y-5">
            <div className="grid grid-cols-2 md:grid-cols-4 gap-3">
              {[
                { icon: Briefcase, label: 'Vacatures', value: bedrijf.vacatures.toString() },
                { icon: Users, label: 'Medewerkers', value: bedrijf.medewerkers.toString() },
                { icon: TrendingUp, label: 'Groei', value: `+${bedrijf.groei}%` },
                { icon: Calendar, label: 'Opgericht', value: bedrijf.opgericht.toString() },
              ].map((item) => (
                <div key={item.label} className="bg-slate-50 border border-slate-100 rounded-lg p-3">
                  <item.icon size={14} className="text-violet-600 mb-1.5" />
                  <p className="text-[10px] text-slate-500 uppercase tracking-wide">{item.label}</p>
                  <p className="text-sm font-semibold text-slate-800 mt-0.5">{item.value}</p>
                </div>
              ))}
            </div>

            <section>
              <h3 className="font-heading font-bold text-slate-800 text-sm mb-2">Over het bedrijf</h3>
              <p className="text-sm text-slate-600 leading-relaxed">
                {bedrijf.naam} is sinds {bedrijf.opgericht} actief in Flevoland en behoort tot de top werkgevers in de {bedrijf.type.toLowerCase()}-sector. Met {bedrijf.medewerkers} medewerkers en een groei van {bedrijf.groei}% in het afgelopen jaar is dit een dynamische werkomgeving voor software developers.
              </p>
            </section>

            <section>
              <h3 className="font-heading font-bold text-slate-800 text-sm mb-3">Tech stack</h3>
              <div className="flex flex-wrap gap-2">
                {bedrijf.techStack.map((tech) => (
                  <span key={tech} className="text-xs px-3 py-1.5 bg-sky-50 text-sky-700 border border-sky-100 rounded-lg font-mono font-semibold">
                    {tech}
                  </span>
                ))}
              </div>
            </section>

            <section>
              <h3 className="font-heading font-bold text-slate-800 text-sm mb-3">Contact</h3>
              <div className="space-y-2">
                <div className="flex items-center gap-2 text-sm text-slate-600">
                  <MapPin size={14} className="text-slate-400" />
                  <span>{bedrijf.stad}, Flevoland</span>
                </div>
                <div className="flex items-center gap-2 text-sm text-slate-600">
                  <Globe size={14} className="text-slate-400" />
                  <span className="font-mono text-xs">www.{bedrijf.naam.toLowerCase().replace(/\s+/g, '')}.nl</span>
                </div>
                <div className="flex items-center gap-2 text-sm text-slate-600">
                  <Mail size={14} className="text-slate-400" />
                  <span className="font-mono text-xs">careers@{bedrijf.naam.toLowerCase().replace(/\s+/g, '')}.nl</span>
                </div>
              </div>
            </section>
          </div>

          <div className="sticky bottom-0 bg-white border-t border-slate-200 px-6 py-4 flex items-center justify-between gap-3">
            <button
              onClick={handleFollow}
              className="px-4 py-2.5 text-sm font-semibold text-slate-700 bg-white border border-slate-200 rounded-lg hover:bg-slate-50 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-sky-500/40"
            >
              Volg bedrijf
            </button>
            <button
              onClick={handleContact}
              className="flex items-center gap-2 px-5 py-2.5 text-sm font-semibold bg-violet-600 text-white rounded-lg hover:bg-violet-700 hover:scale-105 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-violet-500/40"
            >
              <Mail size={14} />
              Neem contact op
            </button>
          </div>
        </motion.div>
      </motion.div>
    </AnimatePresence>
  );
};

export default BedrijfDetailModal;