import React from 'react';
import { ToastContainer } from 'react-toastify';
import 'react-toastify/dist/ReactToastify.css';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import './styles.css';

import Home from './src/pages/Home.vue';
import Vacatures from './src/pages/Vacatures.vue';
import Trends from './src/pages/Trends.vue';
import Admin from './src/pages/Admin.vue';
import Export from './src/pages/Export.vue';
import NotFound from './src/pages/NotFound.vue';
import Regio from './src/pages/Regio.vue';
import Bedrijven from './src/pages/Bedrijven.vue';
import Arbeidsmarkt from './src/pages/Arbeidsmarkt.vue';
import Statistieken from './src/pages/Statistieken.vue';
import Profile from './src/pages/Profile.vue';

const App: React.FC = () => {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/vacatures" element={<Vacatures />} />
        <Route path="/trends" element={<Trends />} />
        <Route path="/regio" element={<Regio />} />
        <Route path="/bedrijven" element={<Bedrijven />} />
        <Route path="/arbeidsmarkt" element={<Arbeidsmarkt />} />
        <Route path="/statistieken" element={<Statistieken />} />
        <Route path="/export" element={<Export />} />
        <Route path="/admin" element={<Admin />} />
        <Route path="/profile" element={<Profile />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
      <ToastContainer
        position="top-right"
        autoClose={3000}
        newestOnTop
        closeOnClick
        pauseOnHover
        toastClassName="text-sm font-medium"
      />
    </Router>
  );
};

export default App;