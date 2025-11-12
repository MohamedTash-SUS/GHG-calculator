import React, { useState, useMemo } from 'react';
import { AreaChart, BarChart, LineChart, Bar, Line, XAxis, YAxis, CartesianGrid, Tooltip, Legend, ResponsiveContainer } from 'recharts';
import { Calculator, Building, Thermometer, AreaChart as AreaChartIcon, BarChart3, HelpCircle, Info, Download, Plus, Trash2, Github, CheckCircle, TrendingUp, TrendingDown } from 'lucide-react';

// --- Sample Data ---
const initialSampleData = [
  { id: '1', year: '2023', month: 'Jan', energy: '120000', hdd: '450', cdd: '10' },
  { id: '2', year: '2023', month: 'Feb', energy: '115000', hdd: '400', cdd: '15' },
  { id: '3', year: '2023', month: 'Mar', energy: '100000', hdd: '300', cdd: '50' },
  { id: '4', year: '2023', month: 'Apr', energy: '85000', hdd: '150', cdd: '100' },
  { id: '5', year: '2023', month: 'May', energy: '90000', hdd: '50', cdd: '200' },
  { id: '6', year: '2023', month: 'Jun', energy: '105000', hdd: '10', cdd: '300' },
  { id: '7', year: '2023', month: 'Jul', energy: '115000', hdd: '0', cdd: '350' },
  { id: '8', year: '2023', month: 'Aug', energy: '110000', hdd: '5', cdd: '320' },
  { id: '9', year: '2023', month: 'Sep', energy: '95000', hdd: '40', cdd: '220' },
  { id: '10', year: '2023', month: 'Oct', energy: '88000', hdd: '120', cdd: '110' },
  { id: '11', year: '2023', month: 'Nov', energy: '102000', hdd: '280', cdd: '40' },
  { id: '12', year: '2023', month: 'Dec', energy: '118000', hdd: '420', cdd: '10' },
  { id: '13', year: '2024', month: 'Jan', energy: '118000', hdd: '460', cdd: '5' },
  { id: '14', year: '2024', month: 'Feb', energy: '112000', hdd: '410', cdd: '20' },
  { id: '15', year: '2024', month: 'Mar', energy: '98000', hdd: '310', cdd: '60' },
  { id: '16', year: '2024', month: 'Apr', energy: '82000', hdd: '140', cdd: '110' },
  { id: '17', year: '2024', month: 'May', energy: '87000', hdd: '60', cdd: '210' },
  { id: '18', year: '2024', month: 'Jun', energy: '102000', hdd: '15', cdd: '310' },
  { id: '19', year: '2024', month: 'Jul', energy: '113000', hdd: '0', cdd: '360' },
  { id: '20', year: '2024', month: 'Aug', energy: '108000', hdd: '10', cdd: '330' },
  { id: '21', year: '2024', month: 'Sep', energy: '93000', hdd: '45', cdd: '230' },
  { id: '22', year: '2024', month: 'Oct', energy: '86000', hdd: '130', cdd: '120' },
  { id: '23', year: '2024', month: 'Nov', energy: '100000', hdd: '290', cdd: '45' },
  { id: '24', year: '2024', month: 'Dec', energy: '116000', hdd: '430', cdd: '15' },
];

// --- Helper Functions ---

/**
 * Performs a simple linear regression
 * @param {Array<Array<number>>} data - Array of [x, y] pairs
 * @returns {object} - { slope, intercept, rSquared }
 */
const simpleLinearRegression = (data) => {
  let sumX = 0, sumY = 0, sumXY = 0, sumXX = 0, sumYY = 0;
  const n = data.length;
  
  if (n === 0) return { slope: 0, intercept: 0, rSquared: 0 };

  for (let i = 0; i < n; i++) {
    const x = data[i][0];
    const y = data[i][1];
    sumX += x;
    sumY += y;
    sumXY += x * y;
    sumXX += x * x;
    sumYY += y * y;
  }

  const denominator = (n * sumXX - sumX * sumX);
  if (denominator === 0) return { slope: 0, intercept: 0, rSquared: 0 };

  const slope = (n * sumXY - sumX * sumY) / denominator;
  const intercept = (sumY - slope * sumX) / n;
  
  const rNum = (n * sumXY - sumX * sumY);
  const rDen = Math.sqrt((n * sumXX - sumX * sumX) * (n * sumYY - sumY * sumY));
  if (rDen === 0) return { slope, intercept, rSquared: 0 };

  const rSquared = Math.pow(rNum / rDen, 2);

  return { slope, intercept, rSquared };
};

/**
 * Formats a number with commas
 * @param {number} value
 * @returns {string}
 */
const formatNumber = (value, decimals = 0) => {
  if (isNaN(value)) return 'N/A';
  return value.toLocaleString(undefined, {
    minimumFractionDigits: decimals,
    maximumFractionDigits: decimals,
  });
};

// --- React Components ---

/**
 * Header Component
 */
const Header = ({ onHelpClick }) => (
  <header className="bg-white shadow-md">
    <div className="container mx-auto px-4 py-4 flex justify-between items-center">
      <div className="flex items-center space-x-3">
        <div className="p-2 bg-blue-600 text-white rounded-lg">
          <Calculator size={24} />
        </div>
        <h1 className="text-2xl font-bold text-gray-800">
          Normalized Building Energy Signature Tracker
        </h1>
      </div>
      <div className="flex items-center space-x-2">
        <a 
          href="https://github.com/google/project-gemini" 
          target="_blank" 
          rel="noopener noreferrer"
          className="p-2 text-gray-600 hover:text-gray-900"
          title="View on GitHub (mock link)"
        >
          <Github size={20} />
        </a>
        <button
          onClick={onHelpClick}
          className="flex items-center space-x-1 px-3 py-2 bg-blue-100 text-blue-700 rounded-lg hover:bg-blue-200 transition-colors"
          title="Help & Documentation"
        >
          <HelpCircle size={18} />
          <span>Help</span>
        </button>
      </div>
    </div>
  </header>
);

/**
 * Help Modal Component
 */
const HelpModal = ({ isVisible, onClose }) => {
  if (!isVisible) return null;

  return (
    <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4">
      <div className="bg-white rounded-lg shadow-xl max-w-2xl w-full max-h-[90vh] overflow-y-auto">
        <div className="flex justify-between items-center p-4 border-b">
          <h2 className="text-xl font-bold text-gray-800 flex items-center">
            <HelpCircle className="mr-2 text-blue-600" />
            Documentation & Concepts
          </h2>
          <button onClick={onClose} className="text-gray-500 hover:text-gray-800">&times;</button>
        </div>
        <div className="p-6 space-y-4">
          <p className="text-gray-700">
            This tool helps you establish a weather-normalized energy baseline for your building, which is essential for effective
            Monitoring, Targeting & Reporting (MT&R) and compliance with standards like ISO 50001.
          </p>

          <div className="space-y-3">
            <h3 className="font-semibold text-gray-800">Key Concepts</h3>
            <ul className="list-disc list-inside space-y-2 text-sm text-gray-600">
              <li>
                <strong>Energy Use Intensity (EUI):</strong> Total energy consumed by a building in a year, divided by its total floor area.
                It's a common metric for benchmarking (e.g., kWh/m²/year).
              </li>
              <li>
                <strong>Weather Normalization:</strong> A statistical process to remove the effect of weather variations on your energy data.
                This allows for a true "apples-to-apples" comparison of energy performance over time.
              </li>
              <li>
                <strong>Degree Days (HDD/CDD):</strong> Heating Degree Days (HDD) and Cooling Degree Days (CDD) are measures of how much
                the outside air temperature was below (HDD) or above (CDD) a specific base temperature (e.g., 18°C or 65°F). They are
                strong indicators of weather-driven energy demand for heating and cooling.
              </li>
              <li>
                <strong>Energy Signature (Regression Model):</strong> This tool uses simple linear regression to find the mathematical
                relationship between energy use and weather. The model used here is:
                <br />
                <code className="block bg-gray-100 p-2 rounded text-sm my-1">
                  Energy = (BLC × (HDD + CDD)) + Baseload
                </code>
                <ul className="list-disc list-inside ml-6 mt-1">
                  <li><strong>BLC (Building Load Coefficient):</strong> The slope of the line. It represents how much energy use (e.g., kWh) increases for each additional Total Degree Day (TDD).</li>
                  <li><strong>Baseload:</strong> The intercept of the line. This is the "weather-independent" energy use—power for lights, computers, ventilation, etc., that runs regardless of the weather.</li>
                </ul>
              </li>
              <li>
                <strong>Energy Baseline (EnB):</strong> A "snapshot" of your building's expected energy use under a set of "normal"
                weather conditions, based on your regression model. This tool uses the average normalized energy over the historical period as the baseline.
              </li>
              <li>
                <strong>Energy Performance Indicator (EnPI):</strong> Your key metric for tracking performance. This tool uses the
                <strong>Weather-Normalized EUI (kWh/m²/year)</strong> as the EnPI. By comparing this year's EnPI to your baseline
                EUI, you can see if your energy efficiency has truly improved or worsened, regardless of whether it was a very hot or cold year.
              </li>
            </ul>
          </div>
        </div>
        <div className="p-4 bg-gray-50 border-t text-right">
          <button
            onClick={onClose}
            className="px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700"
          >
            Got it
          </button>
        </div>
      </div>
    </div>
  );
};

/**
 * Input Section Component
 */
const InputSection = ({ area, setArea, areaUnit, setAreaUnit, energyUnit, setEnergyUnit, data, setData }) => {
  const handleDataChange = (id, field, value) => {
    setData(prevData =>
      prevData.map(row =>
        row.id === id ? { ...row, [field]: value } : row
      )
    );
  };

  const addRow = () => {
    const newId = (Math.max(0, ...data.map(r => parseInt(r.id))) + 1).toString();
    setData(prevData => [
      ...prevData,
      { id: newId, year: '', month: '', energy: '', hdd: '', cdd: '' }
    ]);
  };

  const removeRow = (id) => {
    if (data.length <= 1) return; // Don't remove the last row
    setData(prevData => prevData.filter(row => row.id !== id));
  };
  
  const clearAllData = () => {
     setData([
      { id: '1', year: '', month: '', energy: '', hdd: '', cdd: '' }
    ]);
  };

  return (
    <div className="bg-white p-6 rounded-lg shadow-lg">
      <h2 className="text-xl font-semibold text-gray-800 mb-4 flex items-center">
        <Building className="mr-2 text-blue-600" />
        1. Building & Energy Data Input
      </h2>
      
      {/* --- Step 1: Area & Units --- */}
      <div className="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
        <div>
          <label htmlFor="area" className="block text-sm font-medium text-gray-700">
            Conditioned Area
          </label>
          <div className="mt-1 flex rounded-md shadow-sm">
            <input
              type="number"
              id="area"
              value={area}
              onChange={(e) => setArea(e.target.value)}
              className="flex-1 block w-full rounded-none rounded-l-md border-gray-300 focus:border-blue-500 focus:ring-blue-500 sm:text-sm"
              placeholder="e.g., 10000"
            />
            <select
              value={areaUnit}
              onChange={(e) => setAreaUnit(e.target.value)}
              className="inline-flex items-center rounded-r-md border border-l-0 border-gray-300 bg-gray-50 px-3 text-sm text-gray-500"
            >
              <option value="m²">m²</option>
              <option value="ft²">ft²</option>
            </select>
          </div>
        </div>
        <div>
          <label htmlFor="energyUnit" className="block text-sm font-medium text-gray-700">
            Energy Unit
          </label>
          <select
            id="energyUnit"
            value={energyUnit}
            onChange={(e) => setEnergyUnit(e.target.value)}
            className="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-500 focus:ring-blue-500 sm:text-sm"
          >
            <option>kWh</option>
            <option>GJ</option>
            <option>MWh</option>
            <option>MBtu</option>
          </select>
        </div>
         <div>
          <label htmlFor="weatherUnit" className="block text-sm font-medium text-gray-700">
            Degree Day Base (Info)
          </label>
           <input
              type="text"
              id="weatherUnit"
              disabled
              value="e.g., 18°C or 65°F"
              className="mt-1 block w-full rounded-md border-gray-300 bg-gray-100 sm:text-sm"
            />
        </div>
      </div>
      
      {/* --- Step 2: Monthly Data Table --- */}
      <h3 className="text-lg font-medium text-gray-700 mb-2">
        Monthly Historical Data
      </h3>
      <div className="overflow-x-auto max-h-[400px] overflow-y-auto border rounded-lg">
        <table className="min-w-full divide-y divide-gray-200">
          <thead className="bg-gray-100 sticky top-0">
            <tr>
              <th className="px-3 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Year</th>
              <th className="px-3 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Month</th>
              <th className="px-3 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Energy ({energyUnit})</th>
              <th className="px-3 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">HDD</th>
              <th className="px-3 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">CDD</th>
              <th className="px-3 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Actions</th>
            </tr>
          </thead>
          <tbody className="bg-white divide-y divide-gray-200">
            {data.map(row => (
              <tr key={row.id}>
                <td className="px-3 py-1"><input type="text" value={row.year} onChange={(e) => handleDataChange(row.id, 'year', e.target.value)} className="w-20 rounded border-gray-300 shadow-sm sm:text-sm" /></td>
                <td className="px-3 py-1"><input type="text" value={row.month} onChange={(e) => handleDataChange(row.id, 'month', e.target.value)} className="w-20 rounded border-gray-300 shadow-sm sm:text-sm" /></td>
                <td className="px-3 py-1"><input type="text" value={row.energy} onChange={(e) => handleDataChange(row.id, 'energy', e.target.value)} className="w-28 rounded border-gray-300 shadow-sm sm:text-sm" /></td>
                <td className="px-3 py-1"><input type="text" value={row.hdd} onChange={(e) => handleDataChange(row.id, 'hdd', e.target.value)} className="w-20 rounded border-gray-300 shadow-sm sm:text-sm" /></td>
                <td className="px-3 py-1"><input type="text" value={row.cdd} onChange={(e) => handleDataChange(row.id, 'cdd', e.target.value)} className="w-20 rounded border-gray-300 shadow-sm sm:text-sm" /></td>
                <td className="px-3 py-1">
                  <button onClick={() => removeRow(row.id)} className="text-red-500 hover:text-red-700" title="Remove Row">
                    <Trash2 size={16} />
                  </button>
                </td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
      <div className="flex space-x-2 mt-4">
        <button
          onClick={addRow}
          className="flex items-center space-x-1 px-3 py-2 bg-green-100 text-green-700 rounded-lg hover:bg-green-200 text-sm"
        >
          <Plus size={16} />
          <span>Add Row</span>
        </button>
        <button
          onClick={clearAllData}
          className="flex items-center space-x-1 px-3 py-2 bg-red-100 text-red-700 rounded-lg hover:bg-red-200 text-sm"
        >
          <Trash2 size={16} />
          <span>Clear All</span>
        </button>
      </div>
    </div>
  );
};

/**
 * KPI Card Component
 */
const KpiCard = ({ title, value, unit, description, icon, trend = null }) => (
  <div className="bg-white p-4 rounded-lg shadow-md">
    <div className="flex items-center justify-between mb-1">
      <h3 className="text-sm font-medium text-gray-500 uppercase">{title}</h3>
      <div className="text-blue-600">{icon}</div>
    </div>
    <div className="flex items-baseline space-x-1">
       <span className="text-2xl font-bold text-gray-800">{value}</span>
       <span className="text-sm text-gray-600">{unit}</span>
    </div>
    <p className="text-xs text-gray-500 mt-1">{description}</p>
    {trend && (
      <div className={`mt-2 flex items-center text-xs ${trend.direction === 'up' ? 'text-red-500' : 'text-green-500'}`}>
        {trend.direction === 'up' ? <TrendingUp size={14} className="mr-1" /> : <TrendingDown size={14} className="mr-1" />}
        <span>{trend.value} {trend.text}</span>
      </div>
    )}
  </div>
);

/**
 * Output Section Component
 */
const OutputSection = ({ model, results, areaUnit, energyUnit }) => {
  const [activeTab, setActiveTab] = useState('dashboard');
  
  if (!model || !results) {
    return (
      <div className="bg-white p-6 rounded-lg shadow-lg mt-6 text-center">
        <Info className="mx-auto text-blue-500 h-10 w-10 mb-2" />
        <h3 className="text-lg font-medium text-gray-700">No Results to Display</h3>
        <p className="text-sm text-gray-500">
          Enter your building data and click "Calculate Baseline" to see your energy analysis.
        </p>
      </div>
    );
  }

  const { baselineEUI, annualReport } = results.euiReport;
  const { slope, intercept, rSquared } = model;
  const lastYear = annualReport[annualReport.length - 1];
  const baselineTrend = lastYear ? ((lastYear.normalizedEnPI - baselineEUI) / baselineEUI) * 100 : 0;

  const tabs = [
    { id: 'dashboard', name: 'Dashboard', icon: AreaChartIcon },
    { id: 'model', name: 'Model Details', icon: Calculator },
    { id: 'data', name: 'Data Table', icon: BarChart3 },
  ];

  return (
    <div className="bg-white p-6 rounded-lg shadow-lg mt-6">
      <div className="flex justify-between items-center mb-4">
        <h2 className="text-xl font-semibold text-gray-800 flex items-center">
          <CheckCircle className="mr-2 text-green-600" />
          2. Analysis & Reporting
        </h2>
         <button
            onClick={() => alert("Export functionality would be implemented here, e.g., as CSV.")}
            className="flex items-center space-x-1 px-3 py-2 bg-gray-600 text-white rounded-lg hover:bg-gray-700 text-sm"
          >
            <Download size={16} />
            <span>Export Results</span>
          </button>
      </div>
      
      {/* --- Tabs --- */}
      <div className="border-b border-gray-200">
        <nav className="-mb-px flex space-x-6" aria-label="Tabs">
          {tabs.map(tab => (
            <button
              key={tab.id}
              onClick={() => setActiveTab(tab.id)}
              className={`
                ${activeTab === tab.id
                  ? 'border-blue-500 text-blue-600'
                  : 'border-transparent text-gray-500 hover:text-gray-700 hover:border-gray-300'
                }
                group inline-flex items-center py-3 px-1 border-b-2 font-medium text-sm
              `}
            >
              <tab.icon className="mr-2 h-5 w-5" />
              <span>{tab.name}</span>
            </button>
          ))}
        </nav>
      </div>

      {/* --- Tab Content --- */}
      <div className="mt-6">
        {/* --- Dashboard Tab --- */}
        {activeTab === 'dashboard' && (
          <div className="space-y-6">
            <h3 className="text-lg font-medium text-gray-900">Performance Dashboard</h3>
            {/* KPI Cards */}
            <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
              <KpiCard
                title="Baseline EUI (EnB)"
                value={formatNumber(baselineEUI, 1)}
                unit={`${energyUnit}/${areaUnit}/yr`}
                description="Your official weather-normalized energy baseline."
                icon={<Building size={20} />}
              />
              <KpiCard
                title="Latest Year EnPI"
                value={formatNumber(lastYear.normalizedEnPI, 1)}
                unit={`${energyUnit}/${areaUnit}/yr`}
                description={`Weather-normalized EUI for ${lastYear.year}.`}
                icon={<Thermometer size={20} />}
                trend={{
                  direction: baselineTrend >= 0 ? 'up' : 'down',
                  value: `${Math.abs(baselineTrend).toFixed(1)}%`,
                  text: `${baselineTrend >= 0 ? 'worse' : 'better'} than baseline`
                }}
              />
               <KpiCard
                title="Actual EUI (Latest)"
                value={formatNumber(lastYear.actualEUI, 1)}
                unit={`${energyUnit}/${areaUnit}/yr`}
                description={`Actual (un-normalized) EUI for ${lastYear.year}.`}
                icon={<AreaChartIcon size={20} />}
              />
              <KpiCard
                title="Model Fit (R-Squared)"
                value={formatNumber(rSquared, 2)}
                unit=""
                description="How well weather predicts energy (1.0 = perfect fit)."
                icon={<CheckCircle size={20} />}
              />
            </div>
            
            {/* Charts */}
            <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
              <div className="p-4 border rounded-lg">
                <h4 className="text-md font-semibold text-gray-800 mb-4">Historical vs. Normalized Energy Use</h4>
                <div className="h-80">
                  <ResponsiveContainer width="100%" height="100%">
                    <LineChart data={results.processedData}>
                      <CartesianGrid strokeDasharray="3 3" />
                      <XAxis dataKey="name" fontSize={10} />
                      <YAxis fontSize={10} label={{ value: energyUnit, angle: -90, position: 'insideLeft', offset: -5, fontSize: 10 }}/>
                      <Tooltip />
                      <Legend />
                      <Line type="monotone" dataKey="actualEnergy" name="Actual Energy" stroke="#ef4444" dot={false} />
                      <Line type="monotone" dataKey="normalizedEnergy" name="Normalized (Baseline) Energy" stroke="#3b82f6" strokeDasharray="5 5" dot={false} />
                    </LineChart>
                  </ResponsiveContainer>
                </div>
              </div>
              
              <div className="p-4 border rounded-lg">
                 <h4 className="text-md font-semibold text-gray-800 mb-4">Year-over-Year EUI & EnPI Tracking</h4>
                <div className="h-80">
                  <ResponsiveContainer width="100%" height="100%">
                    <BarChart data={annualReport}>
                      <CartesianGrid strokeDasharray="3 3" />
                      <XAxis dataKey="year" fontSize={10} />
                      <YAxis fontSize={10} label={{ value: `${energyUnit}/${areaUnit}/yr`, angle: -90, position: 'insideLeft', offset: -5, fontSize: 10 }}/>
                      <Tooltip />
                      <Legend />
                      <Bar dataKey="actualEUI" name="Actual EUI" fill="#fca5a5" />
                      <Bar dataKey="normalizedEnPI" name="Normalized EnPI" fill="#60a5fa" />
                    </BarChart>
                  </ResponsiveContainer>
                </div>
              </div>
            </div>
          </div>
        )}

        {/* --- Model Details Tab --- */}
        {activeTab === 'model' && (
           <div className="space-y-4">
            <h3 className="text-lg font-medium text-gray-900">Regression Model Details</h3>
            <p className="text-sm text-gray-600">
              The tool calculated a simple linear regression model based on your data, using Total Degree Days (HDD + CDD) as the
              independent variable and Energy Use as the dependent variable.
            </p>
            <div className="bg-gray-100 p-4 rounded-lg">
              <h4 className="font-semibold">Regression Formula:</h4>
              <p className="font-mono text-lg text-blue-700 mt-1">
                Energy = {formatNumber(slope, 2)} × (HDD + CDD) + {formatNumber(intercept, 0)}
              </p>
            </div>
            <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
              <div className="p-4 border rounded-lg">
                <div className="text-sm font-medium text-gray-500">Building Load Coefficient (BLC)</div>
                <div className="text-2xl font-bold text-gray-800">{formatNumber(slope, 2)}</div>
                <div className="text-xs text-gray-500">{energyUnit} per Total Degree Day</div>
              </div>
              <div className="p-4 border rounded-lg">
                <div className="text-sm font-medium text-gray-500">Baseload (Weather-Independent)</div>
                <div className="text-2xl font-bold text-gray-800">{formatNumber(intercept, 0)}</div>
                <div className="text-xs text-gray-500">{energyUnit} per month</div>
              </div>
              <div className="p-4 border rounded-lg">
                <div className="text-sm font-medium text-gray-500">R-Squared (Model Fit)</div>
                <div className="text-2xl font-bold text-gray-800">{formatNumber(rSquared, 3)}</div>
                <div className="text-xs text-gray-500">({(rSquared * 100).toFixed(1)}% of energy variance explained by weather)</div>
              </div>
            </div>
             <p className="text-xs text-gray-500 italic mt-4">
                <strong>Note:</strong> This is a simple 2-parameter model. For advanced analysis (e.g., ISO 50001 compliance), 
                a 3-parameter change-point model (separating HDD and CDD) is often used. This tool provides a strong
                foundational estimate for MT&R.
              </p>
           </div>
        )}

        {/* --- Data Table Tab --- */}
        {activeTab === 'data' && (
           <div className="space-y-4">
            <h3 className="text-lg font-medium text-gray-900">Processed Data Table</h3>
             <div className="overflow-x-auto max-h-[500px] overflow-y-auto border rounded-lg">
              <table className="min-w-full divide-y divide-gray-200">
                <thead className="bg-gray-100 sticky top-0">
                  <tr>
                    <th className="px-3 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Period</th>
                    <th className="px-3 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">TDD (HDD+CDD)</th>
                    <th className="px-3 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Actual Energy</th>
                    <th className="px-3 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Normalized Energy</th>
                    <th className="px-3 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Savings/Loss</th>
                  </tr>
                </thead>
                <tbody className="bg-white divide-y divide-gray-200">
                  {results.processedData.map(row => (
                    <tr key={row.name}>
                      <td className="px-3 py-2 whitespace-nowrap text-sm">{row.name}</td>
                      <td className="px-3 py-2 whitespace-nowrap text-sm">{formatNumber(row.tdd)}</td>
                      <td className="px-3 py-2 whitespace-nowrap text-sm">{formatNumber(row.actualEnergy)}</td>
                      <td className="px-3 py-2 whitespace-nowrap text-sm">{formatNumber(row.normalizedEnergy, 0)}</td>
                      <td className={`px-3 py-2 whitespace-nowrap text-sm ${row.savings > 0 ? 'text-green-600' : 'text-red-600'}`}>
                        {formatNumber(row.savings, 0)}
                      </td>
                    </tr>
                  ))}
                </tbody>
              </table>
            </div>
           </div>
        )}
      </div>
    </div>
  );
};


/**
 * Main App Component
 */
export default function App() {
  const [area, setArea] = useState('10000');
  const [areaUnit, setAreaUnit] = useState('m²');
  const [energyUnit, setEnergyUnit] = useState('kWh');
  const [data, setData] = useState(initialSampleData);
  
  const [model, setModel] = useState(null);
  const [results, setResults] = useState(null);
  
  const [helpVisible, setHelpVisible] = useState(false);
  const [error, setError] = useState(null);

  const canCalculate = useMemo(() => {
    const numArea = parseFloat(area);
    return !isNaN(numArea) && numArea > 0 && data.length >= 12;
  }, [area, data]);

  const handleCalculate = () => {
    setError(null);
    const numArea = parseFloat(area);
    if (isNaN(numArea) || numArea <= 0) {
      setError("Please enter a valid, positive number for the area.");
      return;
    }

    // 1. Parse and validate data
    const parsedData = data.map(row => ({
      ...row,
      energy: parseFloat(row.energy),
      hdd: parseFloat(row.hdd),
      cdd: parseFloat(row.cdd),
    })).filter(row => 
      !isNaN(row.energy) && !isNaN(row.hdd) && !isNaN(row.cdd) &&
      row.energy > 0
    );

    if (parsedData.length < 12) {
      setError("At least 12 valid data rows are required for a meaningful analysis.");
      return;
    }

    // 2. Create regression pairs [x, y]
    // x = Total Degree Days (TDD), y = Energy
    const regressionPairs = parsedData.map(row => [
      row.hdd + row.cdd,
      row.energy
    ]);
    
    // 3. Perform regression
    const calculatedModel = simpleLinearRegression(regressionPairs);
    if (calculatedModel.rSquared < 0.1) { // Basic sanity check
      console.warn("Model has a very low R-Squared value. Results may not be reliable.");
    }
    setModel(calculatedModel);

    // 4. Process data for charts and EUI
    let totalEnergy = 0;
    let totalNormalizedEnergy = 0;
    const yearData = {};

    const processedData = parsedData.map(row => {
      const tdd = row.hdd + row.cdd;
      const normalizedEnergy = calculatedModel.slope * tdd + calculatedModel.intercept;
      const actualEnergy = row.energy;
      const savings = normalizedEnergy - actualEnergy; // Positive = savings
      
      totalEnergy += actualEnergy;
      totalNormalizedEnergy += normalizedEnergy;
      
      const name = `${row.month} ${row.year}`;
      
      // Group by year for EUI report
      if (!yearData[row.year]) {
        yearData[row.year] = { actualSum: 0, normalizedSum: 0, count: 0 };
      }
      yearData[row.year].actualSum += actualEnergy;
      yearData[row.year].normalizedSum += normalizedEnergy;
      yearData[row.year].count += 1;
      
      return { name, actualEnergy, normalizedEnergy, tdd, savings };
    });

    // 5. Calculate EUI & EnPI
    const numYears = Object.keys(yearData).length;
    
    // Baseline Energy (EnB) is the average *annual* normalized energy
    // We use the sum of all *monthly* normalized energies / numYears
    const baselineAnnualNormalizedEnergy = totalNormalizedEnergy / numYears;
    
    // Baseline EUI is the Baseline Energy / Area
    const baselineEUI = baselineAnnualNormalizedEnergy / numArea;
    
    const annualReport = Object.keys(yearData).sort().map(yearStr => {
      const year = yearData[yearStr];
      // Normalize to a full 12-month year if data is partial
      const annualizationFactor = 12 / year.count;
      
      const actualAnnualEnergy = year.actualSum * annualizationFactor;
      const normalizedAnnualEnergy = year.normalizedSum * annualizationFactor;

      const actualEUI = actualAnnualEnergy / numArea;
      const normalizedEnPI = normalizedAnnualEnergy / numArea; // The EnPI
      
      return { year: yearStr, actualEUI, normalizedEnPI };
    });
    
    setResults({
      processedData,
      euiReport: {
        baselineEUI,
        annualReport
      }
    });
  };

  return (
    <div className="min-h-screen bg-gray-100 font-sans">
      <Header onHelpClick={() => setHelpVisible(true)} />
      
      <main className="container mx-auto p-4 md:p-6 lg:p-8 space-y-6">
        <InputSection
          area={area}
          setArea={setArea}
          areaUnit={areaUnit}
          setAreaUnit={setAreaUnit}
          energyUnit={energyUnit}
          setEnergyUnit={setEnergyUnit}
          data={data}
          setData={setData}
        />
        
        {error && (
          <div className="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded-lg" role="alert">
            <strong className="font-bold">Error: </strong>
            <span className="block sm:inline">{error}</span>
          </div>
        )}
        
        <div className="flex justify-center">
          <button
            onClick={handleCalculate}
            disabled={!canCalculate}
            className="flex items-center space-x-2 px-6 py-3 bg-blue-600 text-white font-semibold rounded-lg shadow-md hover:bg-blue-700 disabled:bg-gray-400 disabled:cursor-not-allowed transition-colors"
          >
            <Calculator size={20} />
            <span>Calculate Baseline & EnPI</span>
          </button>
        </div>
        
        <OutputSection
          model={model}
          results={results}
          areaUnit={areaUnit}
          energyUnit={energyUnit}
        />
      </main>
      
      <HelpModal isVisible={helpVisible} onClose={() => setHelpVisible(false)} />
    </div>
  );
}
