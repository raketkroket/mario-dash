import React, { useState } from 'react';
import { motion } from 'framer-motion';
import { User, Mail, Phone, MapPin, Briefcase, Calendar, Shield, Bell, Save, Camera } from 'lucide-react';
import DashboardLayout from '../components/DashboardLayout.vue';
import { toast } from 'react-toastify';

const Profile: React.FC = () => {
  const [tab, setTab] = useState<'profiel' | 'voorkeuren' | 'beveiliging'>('profiel');
  const [form, setForm] = useState({
    naam: 'Admin Beheerder',
    email: 'admin@flevoland.nl',
    telefoon: '+31 6 12345678',
    functie: 'Dashboard Beheerder',
    stad: 'Almere',
    bio: 'Verantwoordelijk voor het beheer en de analyse van arbeidsmarktdata in de provincie Flevoland.',
  });
  const [voorkeuren, setVoorkeuren] = useState({
    emailMeldingen: true,
    pushMeldingen: false,
    weeklyReport: true,
    dataExport: true,
    taal: 'nl',
    tijdzone: 'Europe/Amsterdam',
  });
  const [beveiliging, setBeveiliging] = useState({
    oudWachtwoord: '',
    nieuwWachtwoord: '',
    bevestigWachtwoord: '',
    tweeFactor: true,
  });

  const handleSaveProfile = (e: React.FormEvent) => {
    e.preventDefault();
    toast.success('Profielgegevens opgeslagen');
  };

  const handleSaveVoorkeuren = (e: React.FormEvent) => {
    e.preventDefault();
    toast.success('Voorkeuren bijgewerkt');
  };

  const handleSaveBeveiliging = (e: React.FormEvent) => {
    e.preventDefault();
    if (beveiliging.nieuwWachtwoord && beveiliging.nieuwWachtwoord !== beveiliging.bevestigWachtwoord) {
      toast.error('Wachtwoorden komen niet overeen');
      return;
    }
    toast.success('Beveiligingsinstellingen opgeslagen');
    setBeveiliging({ ...beveiliging, oudWachtwoord: '', nieuwWachtwoord: '', bevestigWachtwoord: '' });
  };

  const handleAvatar = () => {
    toast.info('Avatar uploaden binnenkort beschikbaar');
  };

  const tabs: { id: typeof tab; label: string; icon: React.ElementType }[] = [
    { id: 'profiel', label: 'Profiel', icon: User },
    { id: 'voorkeuren', label: 'Voorkeuren', icon: Bell },
    { id: 'beveiliging', label: 'Beveiliging', icon: Shield },
  ];

  return (
    <DashboardLayout title="Profiel" subtitle="Beheer je accountgegevens en voorkeuren">
      <div className="space-y-6 max-w-4xl">
        <motion.div
          initial={{ opacity: 0, y: 12 }}
          animate={{ opacity: 1, y: 0 }}
          className="bg-white border border-slate-200 rounded-xl p-6"
        >
          <div className="flex flex-col sm:flex-row items-start sm:items-center gap-5">
            <div className="relative">
              <div className="w-20 h-20 rounded-2xl bg-gradient-to-br from-sky-500 to-sky-600 flex items-center justify-center text-white text-2xl font-heading font-bold shadow-md">
                {form.naam.charAt(0)}
              </div>
              <button
                onClick={handleAvatar}
                aria-label="Wijzig avatar"
                className="absolute -bottom-1 -right-1 w-7 h-7 rounded-full bg-white border border-slate-200 flex items-center justify-center text-slate-500 hover:text-sky-600 hover:bg-sky-50 transition-all duration-200 shadow-sm focus:outline-none focus:ring-2 focus:ring-sky-500/40"
              >
                <Camera size={12} />
              </button>
            </div>
            <div className="flex-1 min-w-0">
              <h2 className="font-heading font-bold text-slate-800 text-lg">{form.naam}</h2>
              <p className="text-sm text-slate-500">{form.functie}</p>
              <div className="flex flex-wrap items-center gap-3 mt-2">
                <span className="flex items-center gap-1 text-xs text-slate-500">
                  <Mail size={11} /> {form.email}
                </span>
                <span className="flex items-center gap-1 text-xs text-slate-500">
                  <MapPin size={11} /> {form.stad}
                </span>
                <span className="text-[10px] px-2 py-0.5 rounded-full font-semibold bg-emerald-100 text-emerald-700">
                  Actief
                </span>
              </div>
            </div>
            <div className="text-right shrink-0 hidden sm:block">
              <p className="text-xs text-slate-400">Lid sinds</p>
              <p className="text-sm font-semibold text-slate-700 flex items-center gap-1">
                <Calendar size={12} /> Jan 2024
              </p>
            </div>
          </div>
        </motion.div>

        <div className="grid grid-cols-2 md:grid-cols-4 gap-3">
          {[
            { label: 'Sessies actief', value: '3' },
            { label: 'Exports deze maand', value: '12' },
            { label: 'Opgeslagen filters', value: '5' },
            { label: 'Beheerde datasets', value: '8' },
          ].map((item) => (
            <div key={item.label} className="bg-white border border-slate-200 rounded-xl p-4">
              <p className="text-xl font-bold font-heading text-slate-800">{item.value}</p>
              <p className="text-xs text-slate-500 mt-0.5">{item.label}</p>
            </div>
          ))}
        </div>

        <div className="bg-white border border-slate-200 rounded-xl overflow-hidden">
          <div className="flex border-b border-slate-200">
            {tabs.map((t) => (
              <button
                key={t.id}
                onClick={() => setTab(t.id)}
                className={`flex-1 flex items-center justify-center gap-2 px-4 py-3 text-sm font-semibold transition-all duration-200 focus:outline-none ${
                  tab === t.id
                    ? 'text-sky-600 border-b-2 border-sky-600 bg-sky-50/50'
                    : 'text-slate-500 hover:text-slate-700 hover:bg-slate-50'
                }`}
              >
                <t.icon size={14} />
                {t.label}
              </button>
            ))}
          </div>

          <div className="p-6">
            {tab === 'profiel' && (
              <form onSubmit={handleSaveProfile} className="space-y-4">
                <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                  <div>
                    <label className="block text-xs font-semibold text-slate-600 mb-1.5">Volledige naam</label>
                    <div className="relative">
                      <User size={14} className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400" />
                      <input
                        type="text"
                        value={form.naam}
                        onChange={(e) => setForm({ ...form, naam: e.target.value })}
                        className="w-full pl-9 pr-3 py-2 text-sm border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200"
                      />
                    </div>
                  </div>
                  <div>
                    <label className="block text-xs font-semibold text-slate-600 mb-1.5">E-mailadres</label>
                    <div className="relative">
                      <Mail size={14} className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400" />
                      <input
                        type="email"
                        value={form.email}
                        onChange={(e) => setForm({ ...form, email: e.target.value })}
                        className="w-full pl-9 pr-3 py-2 text-sm border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200"
                      />
                    </div>
                  </div>
                  <div>
                    <label className="block text-xs font-semibold text-slate-600 mb-1.5">Telefoonnummer</label>
                    <div className="relative">
                      <Phone size={14} className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400" />
                      <input
                        type="tel"
                        value={form.telefoon}
                        onChange={(e) => setForm({ ...form, telefoon: e.target.value })}
                        className="w-full pl-9 pr-3 py-2 text-sm border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200"
                      />
                    </div>
                  </div>
                  <div>
                    <label className="block text-xs font-semibold text-slate-600 mb-1.5">Functie</label>
                    <div className="relative">
                      <Briefcase size={14} className="absolute left-3 top-1/2 -translate-y-1/2 text-slate-400" />
                      <input
                        type="text"
                        value={form.functie}
                        onChange={(e) => setForm({ ...form, functie: e.target.value })}
                        className="w-full pl-9 pr-3 py-2 text-sm border border-slate-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200"
                      />
                    </div>
                  </div>
                  <div className="md:col-span-2">
                    <label className="block text-xs font-semibold text-slate-600 mb-1.5">Stad</label>
                    <select
                      value={form.stad}
                      onChange={(e) => setForm({ ...form, stad: e.target.value })}
                      className="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 bg-white focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200 text-slate-700"
                    >
                      <option>Almere</option>
                      <option>Lelystad</option>
                      <option>Emmeloord</option>
                      <option>Dronten</option>
                      <option>Zeewolde</option>
                      <option>Urk</option>
                    </select>
                  </div>
                  <div className="md:col-span-2">
                    <label className="block text-xs font-semibold text-slate-600 mb-1.5">Bio</label>
                    <textarea
                      rows={3}
                      value={form.bio}
                      onChange={(e) => setForm({ ...form, bio: e.target.value })}
                      className="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200 resize-none"
                    />
                  </div>
                </div>
                <div className="flex justify-end pt-2 border-t border-slate-100">
                  <button
                    type="submit"
                    className="flex items-center gap-2 px-5 py-2.5 text-sm font-semibold bg-sky-600 text-white rounded-lg hover:bg-sky-700 hover:scale-105 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-sky-500/40"
                  >
                    <Save size={14} />
                    Profiel opslaan
                  </button>
                </div>
              </form>
            )}

            {tab === 'voorkeuren' && (
              <form onSubmit={handleSaveVoorkeuren} className="space-y-4">
                <div className="space-y-3">
                  {[
                    { key: 'emailMeldingen', label: 'E-mailmeldingen', sub: 'Ontvang updates over nieuwe vacatures en trends' },
                    { key: 'pushMeldingen', label: 'Push-meldingen', sub: 'Realtime browser-meldingen voor belangrijke updates' },
                    { key: 'weeklyReport', label: 'Wekelijks rapport', sub: 'Ontvang elke maandag een samenvatting per e-mail' },
                    { key: 'dataExport', label: 'Automatische export', sub: 'Maandelijkse CSV-export naar je e-mail' },
                  ].map((item) => {
                    const k = item.key as keyof typeof voorkeuren;
                    const checked = voorkeuren[k] as boolean;
                    return (
                      <label
                        key={item.key}
                        className="flex items-center justify-between gap-4 p-4 bg-slate-50 border border-slate-100 rounded-lg cursor-pointer hover:bg-slate-100/60 transition-all duration-200"
                      >
                        <div className="flex-1 min-w-0">
                          <p className="text-sm font-semibold text-slate-800">{item.label}</p>
                          <p className="text-xs text-slate-500 mt-0.5">{item.sub}</p>
                        </div>
                        <button
                          type="button"
                          onClick={() => setVoorkeuren({ ...voorkeuren, [k]: !checked })}
                          aria-pressed={checked}
                          className={`relative w-11 h-6 rounded-full transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-sky-500/40 ${
                            checked ? 'bg-sky-600' : 'bg-slate-300'
                          }`}
                        >
                          <span
                            className={`absolute top-0.5 left-0.5 w-5 h-5 bg-white rounded-full shadow-sm transition-transform duration-200 ${
                              checked ? 'translate-x-5' : 'translate-x-0'
                            }`}
                          />
                        </button>
                      </label>
                    );
                  })}
                </div>

                <div className="grid grid-cols-1 md:grid-cols-2 gap-4 pt-2">
                  <div>
                    <label className="block text-xs font-semibold text-slate-600 mb-1.5">Taal</label>
                    <select
                      value={voorkeuren.taal}
                      onChange={(e) => setVoorkeuren({ ...voorkeuren, taal: e.target.value })}
                      className="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 bg-white focus:outline-none focus:ring-2 focus:ring-sky-500/40 text-slate-700"
                    >
                      <option value="nl">Nederlands</option>
                      <option value="en">English</option>
                      <option value="de">Deutsch</option>
                    </select>
                  </div>
                  <div>
                    <label className="block text-xs font-semibold text-slate-600 mb-1.5">Tijdzone</label>
                    <select
                      value={voorkeuren.tijdzone}
                      onChange={(e) => setVoorkeuren({ ...voorkeuren, tijdzone: e.target.value })}
                      className="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 bg-white focus:outline-none focus:ring-2 focus:ring-sky-500/40 text-slate-700"
                    >
                      <option value="Europe/Amsterdam">Europe/Amsterdam</option>
                      <option value="Europe/Berlin">Europe/Berlin</option>
                      <option value="Europe/London">Europe/London</option>
                      <option value="UTC">UTC</option>
                    </select>
                  </div>
                </div>

                <div className="flex justify-end pt-2 border-t border-slate-100">
                  <button
                    type="submit"
                    className="flex items-center gap-2 px-5 py-2.5 text-sm font-semibold bg-sky-600 text-white rounded-lg hover:bg-sky-700 hover:scale-105 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-sky-500/40"
                  >
                    <Save size={14} />
                    Voorkeuren opslaan
                  </button>
                </div>
              </form>
            )}

            {tab === 'beveiliging' && (
              <form onSubmit={handleSaveBeveiliging} className="space-y-4">
                <div className="bg-amber-50 border border-amber-200 rounded-lg p-4 flex items-start gap-3">
                  <Shield size={16} className="text-amber-600 shrink-0 mt-0.5" />
                  <div>
                    <p className="text-sm font-semibold text-amber-800">Bescherm je account</p>
                    <p className="text-xs text-amber-700 mt-0.5">
                      Gebruik een sterk wachtwoord met minstens 12 tekens, hoofdletters en cijfers.
                    </p>
                  </div>
                </div>

                <div className="space-y-3">
                  <div>
                    <label className="block text-xs font-semibold text-slate-600 mb-1.5">Huidig wachtwoord</label>
                    <input
                      type="password"
                      value={beveiliging.oudWachtwoord}
                      onChange={(e) => setBeveiliging({ ...beveiliging, oudWachtwoord: e.target.value })}
                      placeholder="••••••••"
                      className="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200"
                    />
                  </div>
                  <div>
                    <label className="block text-xs font-semibold text-slate-600 mb-1.5">Nieuw wachtwoord</label>
                    <input
                      type="password"
                      value={beveiliging.nieuwWachtwoord}
                      onChange={(e) => setBeveiliging({ ...beveiliging, nieuwWachtwoord: e.target.value })}
                      placeholder="••••••••"
                      className="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200"
                    />
                  </div>
                  <div>
                    <label className="block text-xs font-semibold text-slate-600 mb-1.5">Bevestig nieuw wachtwoord</label>
                    <input
                      type="password"
                      value={beveiliging.bevestigWachtwoord}
                      onChange={(e) => setBeveiliging({ ...beveiliging, bevestigWachtwoord: e.target.value })}
                      placeholder="••••••••"
                      className="w-full text-sm border border-slate-200 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-sky-500/40 focus:border-sky-400 transition-all duration-200"
                    />
                  </div>
                </div>

                <label className="flex items-center justify-between gap-4 p-4 bg-slate-50 border border-slate-100 rounded-lg cursor-pointer">
                  <div className="flex-1 min-w-0">
                    <p className="text-sm font-semibold text-slate-800">Twee-factor authenticatie</p>
                    <p className="text-xs text-slate-500 mt-0.5">Extra beveiligingslaag via authenticator-app</p>
                  </div>
                  <button
                    type="button"
                    onClick={() => setBeveiliging({ ...beveiliging, tweeFactor: !beveiliging.tweeFactor })}
                    aria-pressed={beveiliging.tweeFactor}
                    className={`relative w-11 h-6 rounded-full transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-sky-500/40 ${
                      beveiliging.tweeFactor ? 'bg-sky-600' : 'bg-slate-300'
                    }`}
                  >
                    <span
                      className={`absolute top-0.5 left-0.5 w-5 h-5 bg-white rounded-full shadow-sm transition-transform duration-200 ${
                        beveiliging.tweeFactor ? 'translate-x-5' : 'translate-x-0'
                      }`}
                    />
                  </button>
                </label>

                <div className="bg-white border border-slate-200 rounded-lg p-4">
                  <p className="text-sm font-semibold text-slate-800 mb-3">Recente sessies</p>
                  <div className="space-y-2">
                    {[
                      { device: 'Chrome op Windows', locatie: 'Almere, NL', tijd: 'Nu actief', current: true },
                      { device: 'Safari op iPhone', locatie: 'Lelystad, NL', tijd: '2 uur geleden', current: false },
                      { device: 'Firefox op Mac', locatie: 'Amsterdam, NL', tijd: '3 dagen geleden', current: false },
                    ].map((s) => (
                      <div key={s.device} className="flex items-center justify-between text-xs py-2 border-b border-slate-50 last:border-0">
                        <div>
                          <p className="font-semibold text-slate-700">{s.device}</p>
                          <p className="text-slate-400 mt-0.5">{s.locatie} · {s.tijd}</p>
                        </div>
                        {s.current ? (
                          <span className="text-[10px] px-2 py-0.5 rounded-full font-semibold bg-emerald-100 text-emerald-700">
                            Huidige sessie
                          </span>
                        ) : (
                          <button
                            type="button"
                            onClick={() => toast.success('Sessie uitgelogd')}
                            className="text-[10px] font-semibold text-rose-600 hover:text-rose-700 transition-colors duration-200"
                          >
                            Uitloggen
                          </button>
                        )}
                      </div>
                    ))}
                  </div>
                </div>

                <div className="flex justify-end pt-2 border-t border-slate-100">
                  <button
                    type="submit"
                    className="flex items-center gap-2 px-5 py-2.5 text-sm font-semibold bg-sky-600 text-white rounded-lg hover:bg-sky-700 hover:scale-105 transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-sky-500/40"
                  >
                    <Save size={14} />
                    Wijzigingen opslaan
                  </button>
                </div>
              </form>
            )}
          </div>
        </div>
      </div>
    </DashboardLayout>
  );
};

export default Profile;