# petrol-pump-management
petrol pump management system
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HP Center Mahud | Enterprise PPMS</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/3.9.1/chart.min.js"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        :root { --hp-blue: #0054A6; --hp-red: #ED1C24; --hp-dark: #003366; }
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        .hp-blue-bg { background-color: var(--hp-blue); }
        .hp-red-bg { background-color: var(--hp-red); }
        .sidebar-link { transition: all 0.3s; border-left: 4px solid transparent; }
        .sidebar-link.active { background: rgba(255,255,255,0.1); border-left: 4px solid var(--hp-red); color: white !important; }
        .content-section { display: none; animation: fadeIn 0.4s; }
        .content-section.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        @media print { .no-print { display: none; } .print-area { display: block !important; width: 100%; } }
        
        /* Modal Styles */
        .admin-modal { display: none; position: fixed; z-index: 1000; left: 0; top: 0; width: 100%; height: 100%; background-color: rgba(0, 0, 0, 0.8); }
        .admin-modal.active { display: flex; align-items: center; justify-content: center; }
        .modal-content { position: relative; max-width: 90%; max-height: 90%; }
        .modal-content img { width: 100%; height: auto; border-radius: 10px; box-shadow: 0 0 30px rgba(0, 0, 0, 0.5); }
        .modal-close { position: absolute; top: -40px; right: 0; color: white; font-size: 28px; font-weight: bold; cursor: pointer; }
        .modal-close:hover { color: #ED1C24; }
    </style>
</head>
<body class="bg-gray-100 flex min-h-screen">

    <aside id="mobileSidebar" class="w-72 hp-blue-bg text-white flex-shrink-0 hidden md:flex flex-col no-print shadow-2xl fixed md:relative h-screen md:h-auto z-50 md:z-auto overflow-y-auto">
    <img src="https://upload.wikimedia.org/wikipedia/en/thumb/5/5e/Hindustan_Petroleum_Logo.svg/1200px-Hindustan_Petroleum_Logo.svg.png" alt="HP Logo" class="mx-auto my-4" />
    <div class="admin-name text-center text-xl font-bold">Admin: SAKIB</div>

        <div class="p-6 border-b border-blue-900 flex flex-col items-center">
            <img src="https://upload.wikimedia.org/wikipedia/en/thumb/5/5e/Hindustan_Petroleum_Logo.svg/1200px-Hindustan_Petroleum_Logo.svg.png" 
                 alt="HP Logo" class="h-20 mb-3 bg-white p-2 rounded-xl shadow-md">
            <h2 class="text-lg font-black tracking-tight text-center">HP CENTER, MAHUD</h2>
            <p class="text-[10px] uppercase tracking-widest text-blue-300">Retail Management System</p>
        </div>
        
        <nav class="mt-4 flex-1">
            <a href="javascript:void(0)" onclick="switchTab('dashboard')" class="sidebar-link active flex items-center px-8 py-4 text-blue-100 hover:bg-white/5">
                <i class="fas fa-th-large w-8"></i> <span class="font-bold">Dashboard</span>
            </a>
            <a href="javascript:void(0)" onclick="switchTab('sales')" class="sidebar-link flex items-center px-8 py-4 text-blue-100 hover:bg-white/5">
                <i class="fas fa-gas-pump w-8"></i> <span class="font-bold">Nozzle Sales</span>
            </a>
            <a href="javascript:void(0)" onclick="switchTab('billing')" class="sidebar-link flex items-center px-8 py-4 text-blue-100 hover:bg-white/5">
                <i class="fas fa-receipt w-8"></i> <span class="font-bold">Quick Billing</span>
            </a>
            <a href="javascript:void(0)" onclick="switchTab('inventory')" class="sidebar-link flex items-center px-8 py-4 text-blue-100 hover:bg-white/5">
                <i class="fas fa-database w-8"></i> <span class="font-bold">Stock/Dip</span>
            </a>
            <a href="javascript:void(0)" onclick="switchTab('staff')" class="sidebar-link flex items-center px-8 py-4 text-blue-100 hover:bg-white/5">
                <i class="fas fa-user-shield w-8"></i> <span class="font-bold">Staff Mgmt</span>
            </a>
            <a href="javascript:void(0)" onclick="switchTab('reports')" class="sidebar-link flex items-center px-8 py-4 text-blue-100 hover:bg-white/5">
                <i class="fas fa-file-invoice w-8"></i> <span class="font-bold">MIS Reports</span>
            </a>
        </nav>

        <div class="p-6 bg-black/10 text-[11px] text-blue-200">
            <p><i class="fas fa-phone mr-2"></i> +91 9209320133</p>
            <p class="mt-1"><i class="fas fa-map-marker-alt mr-2"></i> Sangola, MH</p>
        </div>
    </aside>

    <main class="flex-1 flex flex-col h-screen overflow-hidden">
        
        <header class="h-16 bg-white shadow-sm flex items-center justify-between px-8 z-10 no-print">
            <div class="flex items-center gap-4">
                <button class="md:hidden text-hp-blue text-xl" onclick="toggleMobileSidebar()"><i class="fas fa-bars"></i></button>
                <h1 id="page-title" class="text-xl font-black text-gray-800 tracking-tight uppercase">Dashboard</h1>
            </div>
            <div class="flex items-center gap-6">
                <div class="hidden sm:block text-right">
                    <p id="clock-date" class="text-[10px] font-bold text-gray-400 uppercase tracking-widest"></p>
                    <p id="clock-time" class="text-sm font-black text-hp-blue"></p>
                </div>
                <div class="flex items-center gap-3 pl-6 border-l">
                    <img src="admin-profile.jpg" alt="Admin" class="h-10 w-10 rounded-full object-cover shadow-md cursor-pointer hover:opacity-80 transition-opacity" onclick="openAdminModal()">
                    <span class="text-sm font-bold text-gray-700">Admin_Mahud</span>
                </div>
            </div>
        </header>

        <div class="flex-1 overflow-y-auto p-4 md:p-8">

            <section id="dashboard" class="content-section active">
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                    <div class="bg-white p-6 rounded-2xl shadow-sm border-b-4 border-hp-blue">
                        <p class="text-[10px] font-bold text-gray-400 uppercase">Today's Sales</p>
                        <h3 class="text-2xl font-black text-gray-800">₹ 1,42,550</h3>
                        <p class="text-xs text-green-600 font-bold mt-2"><i class="fas fa-caret-up"></i> +8.4%</p>
                    </div>
                    <div class="bg-white p-6 rounded-2xl shadow-sm border-b-4 border-hp-red">
                        <p class="text-[10px] font-bold text-gray-400 uppercase">MS (Petrol) Ltrs</p>
                        <h3 class="text-2xl font-black text-gray-800">4,250 L</h3>
                        <div class="w-full bg-gray-100 h-1.5 mt-3 rounded-full"><div class="bg-hp-red h-1.5 rounded-full" style="width: 45%"></div></div>
                    </div>
                    <div class="bg-white p-6 rounded-2xl shadow-sm border-b-4 border-orange-400">
                        <p class="text-[10px] font-bold text-gray-400 uppercase">HSD (Diesel) Ltrs</p>
                        <h3 class="text-2xl font-black text-gray-800">11,800 L</h3>
                        <div class="w-full bg-gray-100 h-1.5 mt-3 rounded-full"><div class="bg-orange-400 h-1.5 rounded-full" style="width: 75%"></div></div>
                    </div>
                    <div class="bg-white p-6 rounded-2xl shadow-sm border-b-4 border-green-500">
                        <p class="text-[10px] font-bold text-gray-400 uppercase">Active Staff</p>
                        <h3 class="text-2xl font-black text-gray-800">04 / 12</h3>
                        <p class="text-xs text-gray-500 font-bold mt-2">Shift: Morning</p>
                    </div>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                    <div class="bg-white p-6 rounded-2xl shadow-sm border border-gray-100">
                        <h4 class="font-black text-sm text-hp-blue mb-4 uppercase italic">Weekly Sales Performance</h4>
                        <canvas id="mainChart" height="220"></canvas>
                    </div>
                    <div class="bg-white p-6 rounded-2xl shadow-sm border border-gray-100">
                        <h4 class="font-black text-sm text-hp-blue mb-4 uppercase italic">Staff Performance (Current Shift)</h4>
                        <div class="space-y-4">
                            <div class="flex justify-between items-center p-3 bg-gray-50 rounded-xl">
                                <span class="text-sm font-bold">Sakib Ganihar (N1)</span>
                                <span class="text-xs font-black bg-blue-100 text-blue-700 px-3 py-1 rounded-full">₹ 42,400</span>
                            </div>
                            <div class="flex justify-between items-center p-3 bg-gray-50 rounded-xl">
                                <span class="text-sm font-bold">Navid Patel (N2)</span>
                                <span class="text-xs font-black bg-blue-100 text-blue-700 px-3 py-1 rounded-full">₹ 38,150</span>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <section id="sales" class="content-section">
                <div class="bg-white rounded-2xl shadow-sm border overflow-hidden">
                    <div class="p-6 bg-gray-50 border-b flex justify-between">
                        <h3 class="font-bold text-gray-700 italic">Nozzle Reading Entry (Daily Log)</h3>
                        <span class="text-xs font-bold text-hp-red italic tracking-widest uppercase">Verified by ATG System</span>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-left">
                            <thead class="bg-gray-100 text-[10px] uppercase font-bold text-gray-500">
                                <tr>
                                    <th class="p-4">Nozzle ID</th>
                                    <th class="p-4">Opening (L)</th>
                                    <th class="p-4">Closing (L)</th>
                                    <th class="p-4">Testing (L)</th>
                                    <th class="p-4">Sale Vol</th>
                                    <th class="p-4">Rate (₹)</th>
                                    <th class="p-4">Net Total (₹)</th>
                                </tr>
                            </thead>
                            <tbody class="divide-y text-sm font-medium">
                                <tr>
                                    <td class="p-4 font-black">N1 - Petrol</td>
                                    <td class="p-4"><input type="number" class="w-24 border p-1 rounded bg-gray-50" value="45210" disabled></td>
                                    <td class="p-4"><input type="number" class="w-24 border p-1 rounded border-hp-blue focus:ring-2" placeholder="0.0"></td>
                                    <td class="p-4"><input type="number" class="w-16 border p-1 rounded" value="5"></td>
                                    <td class="p-4 text-blue-600 font-bold italic">0.00</td>
                                    <td class="p-4 text-gray-500 font-bold italic">104.20</td>
                                    <td class="p-4 font-black">₹ 0.00</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <div class="p-6 border-t flex justify-end">
                        <button class="hp-blue-bg text-white px-8 py-3 rounded-xl font-black shadow-lg hover:scale-105 transition-transform uppercase">Submit Readings</button>
                    </div>
                </div>
            </section>

            <section id="billing" class="content-section">
                <div class="max-w-4xl mx-auto grid grid-cols-1 md:grid-cols-2 gap-8">
                    <div class="bg-white p-8 rounded-2xl shadow-xl border-t-8 border-hp-red">
                        <h3 class="text-xl font-black mb-6 uppercase tracking-tighter">Retail Invoice Entry</h3>
                        <div class="space-y-5">
                            <div>
                                <label class="text-[10px] font-black text-gray-400 uppercase">Product Select</label>
                                <div class="grid grid-cols-2 gap-2 mt-1">
                                    <button class="py-3 border-2 border-hp-blue bg-blue-50 text-hp-blue font-black rounded-xl">PETROL</button>
                                    <button class="py-3 border-2 border-gray-100 text-gray-400 font-black rounded-xl">DIESEL</button>
                                </div>
                            </div>
                            <div class="grid grid-cols-2 gap-4">
                                <div>
                                    <label class="text-[10px] font-black text-gray-400 uppercase">Sale Amount (₹)</label>
                                    <input type="number" id="bill-amt" oninput="calcBill()" class="w-full text-2xl font-black p-4 border-2 rounded-xl border-gray-100 focus:border-hp-blue" placeholder="0.00">
                                </div>
                                <div>
                                    <label class="text-[10px] font-black text-gray-400 uppercase">Volume (Ltr)</label>
                                    <input type="number" id="bill-qty" class="w-full text-2xl font-black p-4 border-2 rounded-xl bg-gray-50" readonly>
                                </div>
                            </div>
                            <button onclick="window.print()" class="w-full hp-blue-bg text-white font-black py-5 rounded-2xl shadow-xl hover:opacity-90 active:scale-95 transition-all text-xl">
                                <i class="fas fa-print mr-2"></i> PRINT & SAVE
                            </button>
                        </div>
                    </div>
                    
                    <div class="print-area bg-[#fffffe] border p-8 shadow-inner flex flex-col font-mono text-xs text-gray-700">
                        <div class="text-center mb-4">
                            <img src="https://upload.wikimedia.org/wikipedia/en/thumb/5/5e/Hindustan_Petroleum_Logo.svg/1200px-Hindustan_Petroleum_Logo.svg.png" class="h-8 mx-auto mb-2 opacity-50 grayscale">
                            <h4 class="font-black text-sm">HINDUSTAN PETROLEUM CENTER</h4>
                            <p class="text-[10px]">Mahud-Sangola Road, Maharashtra 413307</p>
                            <p class="text-[10px]">GSTIN: 27AAAAA0000A1Z5</p>
                        </div>
                        <div class="border-y border-dashed py-2 mb-4">
                            <div class="flex justify-between"><span>DATE:</span> <span id="bill-date">19/01/2026</span></div>
                            <div class="flex justify-between font-bold"><span>INV NO:</span> <span>HP-MAHUD-8921</span></div>
                        </div>
                        <div class="space-y-1 mb-4 flex-1">
                            <div class="flex justify-between"><span>Product:</span> <span class="font-bold">PETROL (MS)</span></div>
                            <div class="flex justify-between"><span>Density:</span> <span>742.5 kg/m3</span></div>
                            <div class="flex justify-between"><span>Rate/L:</span> <span>₹ 104.20</span></div>
                            <div class="flex justify-between"><span>Volume:</span> <span id="prev-qty" class="font-bold">0.00 L</span></div>
                        </div>
                        <div class="border-t-2 border-double pt-2 text-lg font-black flex justify-between items-center text-black">
                            <span>TOTAL:</span>
                            <span id="prev-total">₹ 0.00</span>
                        </div>
                        <p class="mt-6 text-center italic text-[9px]">Thank you! Save Fuel, Save Money.</p>
                    </div>
                </div>
            </section>

            <section id="inventory" class="content-section">
                <div class="bg-white p-8 rounded-2xl shadow-sm border">
                    <h3 class="text-lg font-black text-hp-blue mb-6 italic uppercase tracking-widest"><i class="fas fa-database mr-2"></i> Tank Inventory Management</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                        <div class="p-6 border-l-4 border-hp-red bg-gray-50 rounded-xl">
                            <p class="text-sm font-black text-hp-red mb-3 underline">TANK 01: MS (PETROL)</p>
                            <div class="grid grid-cols-2 gap-4">
                                <div><label class="text-[10px] font-bold">Dip Reading (mm)</label><input type="number" class="w-full border p-2 rounded" value="1420"></div>
                                <div><label class="text-[10px] font-bold">Water Reading (mm)</label><input type="number" class="w-full border p-2 rounded" value="0"></div>
                            </div>
                            <p class="mt-4 text-sm font-black text-blue-900">Calculated Stock: 4,120.40 Liters</p>
                        </div>
                        <div class="p-6 border-l-4 border-orange-500 bg-gray-50 rounded-xl">
                            <p class="text-sm font-black text-orange-600 mb-3 underline">TANK 02: HSD (DIESEL)</p>
                            <div class="grid grid-cols-2 gap-4">
                                <div><label class="text-[10px] font-bold">Dip Reading (mm)</label><input type="number" class="w-full border p-2 rounded" value="2850"></div>
                                <div><label class="text-[10px] font-bold">Water Reading (mm)</label><input type="number" class="w-full border p-2 rounded" value="0"></div>
                            </div>
                            <p class="mt-4 text-sm font-black text-blue-900">Calculated Stock: 11,840.10 Liters</p>
                        </div>
                    </div>
                </div>
            </section>

            <section id="staff" class="content-section">
                <div class="bg-white rounded-2xl shadow-sm border overflow-hidden">
                    <div class="p-6 bg-hp-blue text-white flex justify-between items-center">
                        <h3 class="font-black italic uppercase tracking-widest"><i class="fas fa-users-cog mr-2"></i> Employee Shift Control</h3>
                        <button class="bg-white text-hp-blue px-4 py-2 rounded-lg text-xs font-black">+ ADD EMPLOYEE</button>
                    </div>
                    <div class="p-8 grid grid-cols-1 md:grid-cols-4 gap-6">
                        <div class="border p-5 rounded-2xl shadow-sm text-center relative overflow-hidden">
                            <div class="absolute top-0 right-0 p-2 bg-green-100 text-green-600 text-[10px] font-black">ACTIVE</div>
                            <img src="https://ui-avatars.com/api/?name=Sakib+Ganihar&background=0054A6&color=fff" class="h-16 w-16 rounded-full mx-auto mb-3 shadow-md">
                            <h4 class="font-black text-gray-800">Sakib Ganihar</h4>
                            <p class="text-xs text-gray-500 mb-4">Night Shift</p>
                            <div class="flex justify-between text-[10px] border-t pt-4">
                                <span>Sales (Today): <b class="text-black">420L</b></span>
                                <span>Nozzle: <b class="text-black">N1</b></span>
                            </div>
                        </div>
                        <div class="border p-5 rounded-2xl shadow-sm text-center relative overflow-hidden">
                            <div class="absolute top-0 right-0 p-2 bg-green-100 text-green-600 text-[10px] font-black">ACTIVE</div>
                            <img src="https://ui-avatars.com/api/?name=Prakash+Solankar&background=0054A6&color=fff" class="h-16 w-16 rounded-full mx-auto mb-3 shadow-md">
                            <h4 class="font-black text-gray-800">Prakash Solankar</h4>
                            <p class="text-xs text-gray-500 mb-4">Night Shift</p>
                            <div class="flex justify-between text-[10px] border-t pt-4">
                                <span>Sales (Today): <b class="text-black">380L</b></span>
                                <span>Nozzle: <b class="text-black">N2</b></span>
                            </div>
                        </div>
                        <div class="border p-5 rounded-2xl shadow-sm text-center relative overflow-hidden">
                            <div class="absolute top-0 right-0 p-2 bg-blue-100 text-blue-600 text-[10px] font-black">ON-DUTY</div>
                            <img src="https://ui-avatars.com/api/?name=Navid+Patel&background=0054A6&color=fff" class="h-16 w-16 rounded-full mx-auto mb-3 shadow-md">
                            <h4 class="font-black text-gray-800">Navid Patel</h4>
                            <p class="text-xs text-gray-500 mb-4">Morning Shift</p>
                            <div class="flex justify-between text-[10px] border-t pt-4">
                                <span>Sales (Today): <b class="text-black">350L</b></span>
                                <span>Nozzle: <b class="text-black">N3</b></span>
                            </div>
                        </div>
                        <div class="border p-5 rounded-2xl shadow-sm text-center relative overflow-hidden">
                            <div class="absolute top-0 right-0 p-2 bg-blue-100 text-blue-600 text-[10px] font-black">ON-DUTY</div>
                            <img src="https://ui-avatars.com/api/?name=Rahul+Shinde&background=0054A6&color=fff" class="h-16 w-16 rounded-full mx-auto mb-3 shadow-md">
                            <h4 class="font-black text-gray-800">Rahul Shinde</h4>
                            <p class="text-xs text-gray-500 mb-4">Morning Shift</p>
                            <div class="flex justify-between text-[10px] border-t pt-4">
                                <span>Sales (Today): <b class="text-black">400L</b></span>
                                <span>Nozzle: <b class="text-black">N4</b></span>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <section id="reports" class="content-section">
                <div class="bg-white p-8 rounded-2xl shadow-sm border mb-8">
                    <h3 class="text-lg font-black text-hp-red mb-6 italic uppercase tracking-widest border-b pb-2">Business Analysis Center</h3>
                    <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
                        <div class="space-y-1">
                            <label class="text-[10px] font-black text-gray-400">START DATE</label>
                            <input type="date" class="w-full border p-3 rounded-xl bg-gray-50 font-bold" value="2026-01-01">
                        </div>
                        <div class="space-y-1">
                            <label class="text-[10px] font-black text-gray-400">END DATE</label>
                            <input type="date" class="w-full border p-3 rounded-xl bg-gray-50 font-bold" value="2026-01-19">
                        </div>
                        <div class="space-y-1">
                            <label class="text-[10px] font-black text-gray-400">REPORT TYPE</label>
                            <select class="w-full border p-3 rounded-xl bg-gray-50 font-bold">
                                <option>Daily Sales MIS</option>
                                <option>Inventory Reconciliation</option>
                                <option>Loss/Gain (Vapor)</option>
                                <option>Shift Wise Settlement</option>
                            </select>
                        </div>
                        <div class="flex items-end">
                            <button onclick="alert('Exporting PDF...')" class="w-full hp-red-bg text-white py-3.5 rounded-xl font-black shadow-lg uppercase hover:opacity-90">
                                <i class="fas fa-file-pdf mr-2"></i> EXPORT MIS
                            </button>
                        </div>
                    </div>
                </div>
                
                <div class="bg-white rounded-2xl shadow-lg border-2 border-gray-50 overflow-hidden">
                    <table class="w-full text-xs">
                        <thead class="bg-hp-blue text-white">
                            <tr>
                                <th class="p-4 text-left">Date</th>
                                <th class="p-4 text-center">MS Sales (L)</th>
                                <th class="p-4 text-center">HSD Sales (L)</th>
                                <th class="p-4 text-center">Revenue (₹)</th>
                                <th class="p-4 text-center">Digital (₹)</th>
                                <th class="p-4 text-center">Shortage (₹)</th>
                            </tr>
                        </thead>
                        <tbody class="divide-y font-bold text-gray-600">
                            <tr class="hover:bg-blue-50">
                                <td class="p-4">18 Jan 2026</td>
                                <td class="p-4 text-center">450.20</td>
                                <td class="p-4 text-center">920.40</td>
                                <td class="p-4 text-center">1,32,450.00</td>
                                <td class="p-4 text-center">98,200.00</td>
                                <td class="p-4 text-center text-red-600">0.00</td>
                            </tr>
                            <tr class="hover:bg-blue-50">
                                <td class="p-4">19 Jan 2026</td>
                                <td class="p-4 text-center">410.50</td>
                                <td class="p-4 text-center">880.10</td>
                                <td class="p-4 text-center">1,22,180.00</td>
                                <td class="p-4 text-center">85,400.00</td>
                                <td class="p-4 text-center text-red-600">-140.00</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </section>

        </div>
    </main>
</div>

<!-- Mobile Overlay -->
<div id="mobileOverlay" class="hidden fixed inset-0 bg-black/50 md:hidden z-40" onclick="closeMobileSidebar()"></div>

<!-- Admin Profile Modal -->
<div id="adminModal" class="admin-modal" onclick="closeAdminModal(event)">
    <div class="modal-content" onclick="event.stopPropagation()">
        <span class="modal-close" onclick="closeAdminModal()">&times;</span>
        <img src="admin-profile.jpg" alt="Admin Profile">
    </div>
</div>

<script>
    // Mobile Menu Toggle
    function toggleMobileSidebar() {
        const sidebar = document.getElementById('mobileSidebar');
        const overlay = document.getElementById('mobileOverlay');
        sidebar.classList.toggle('hidden');
        overlay.classList.toggle('hidden');
    }
    
    function closeMobileSidebar() {
        document.getElementById('mobileSidebar').classList.add('hidden');
        document.getElementById('mobileOverlay').classList.add('hidden');
    }
    
    // Admin Modal Functions
    function openAdminModal() {
        document.getElementById('adminModal').classList.add('active');
    }
    
    function closeAdminModal(event) {
        if (event && event.target.id !== 'adminModal') return;
        document.getElementById('adminModal').classList.remove('active');
    }
    
    // Close modal on Escape key
    document.addEventListener('keydown', function(e) {
        if (e.key === 'Escape') {
            closeAdminModal();
        }
    });

    // Tab Switching Logic
    function switchTab(id) {
        document.querySelectorAll('.content-section').forEach(s => s.classList.remove('active'));
        document.querySelectorAll('.sidebar-link').forEach(l => l.classList.remove('active'));
        
        document.getElementById(id).classList.add('active');
        const activeLink = Array.from(document.querySelectorAll('.sidebar-link'))
            .find(l => l.getAttribute('onclick').includes(id));
        if(activeLink) activeLink.classList.add('active');
        
        document.getElementById('page-title').innerText = id.replace('-', ' ');
        closeMobileSidebar();
    }

    // Real-time Billing Logic
    function calcBill() {
        const rate = 104.20; // Example Petrol Rate
        const amount = parseFloat(document.getElementById('bill-amt').value) || 0;
        const qty = amount > 0 ? (amount / rate).toFixed(2) : "0.00";
        
        document.getElementById('bill-qty').value = qty;
        document.getElementById('prev-qty').innerText = qty + " L";
        document.getElementById('prev-total').innerText = "₹ " + amount.toLocaleString('en-IN');
    }

    // Time & Date Updates
    function updateClock() {
        const now = new Date();
        document.getElementById('clock-date').innerText = now.toLocaleDateString('en-GB', { day: '2-digit', month: 'short', year: 'numeric' });
        document.getElementById('clock-time').innerText = now.toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit', second: '2-digit' });
        document.getElementById('bill-date').innerText = now.toLocaleDateString('en-GB');
    }
    setInterval(updateClock, 1000);
    updateClock();

    // Chart Initialization
    const ctx = document.getElementById('mainChart').getContext('2d');
    new Chart(ctx, {
        type: 'line',
        data: {
            labels: ['13 Jan', '14 Jan', '15 Jan', '16 Jan', '17 Jan', '18 Jan', '19 Jan'],
            datasets: [{
                label: 'Revenue',
                data: [120, 145, 135, 180, 165, 200, 195],
                borderColor: '#0054A6',
                backgroundColor: 'rgba(0, 84, 166, 0.1)',
                tension: 0.4,
                fill: true,
                borderWidth: 4,
                pointRadius: 4,
                pointBackgroundColor: '#ED1C24'
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: { legend: { display: false } },
            scales: { y: { display: false }, x: { grid: { display: false } } }
        }
    });
</script>
</body>
</html>
