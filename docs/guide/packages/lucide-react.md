    {/* HEADER */}
    <header className="bg-white rounded-2xl shadow-sm p-6 mb-6 border border-slate-200">
      <div className="flex justify-between items-center flex-wrap gap-4">
        <div>
          <h1 className="text-2xl font-black text-blue-800 uppercase tracking-tight">Via Talentum Academy</h1>
          <p className="text-slate-500 font-bold text-sm">Módulo: Álgebra Simplificada</p>
        </div>
        <div className="flex items-center gap-4">
          <div className="bg-blue-800 text-white px-5 py-2 rounded-xl font-black shadow-lg shadow-blue-100">
            SCORE: {score}
          </div>
          <button onClick={resetAll} className="p-2 hover:bg-red-50 rounded-lg text-slate-400 hover:text-red-500 transition-colors">
            <RefreshCw size={20}/>
          </button>
        </div>
      </div>

      <nav className="flex flex-wrap gap-2 mt-6">
        {[
          { id: 'intro', label: 'Inicio', icon: <BookOpen size={16}/> },
          { id: 'components', label: 'Componentes', icon: <HelpCircle size={16}/> },
          { id: 'similar', label: 'T. Semejantes', icon: <Star size={16}/> },
          { id: 'reduction', label: 'Reducción', icon: <ArrowRight size={16}/> },
          { id: 'calculator', label: 'Calculadora', icon: <Calculator size={16}/> },
          { id: 'quiz', label: 'Final', icon: <Trophy size={16}/> }
        ].map(item => (
          <button
            key={item.id}
            onClick={() => setCurrentSection(item.id)}
            className={`flex items-center gap-2 px-4 py-2 rounded-lg text-sm font-black transition-all ${
              currentSection === item.id ? 'bg-blue-800 text-white' : 'bg-slate-100 text-slate-600 hover:bg-slate-200'
            }`}
          >
            {item.icon} {item.label}
          </button>
        ))}
      </nav>
    </header>

    {/* CONTENIDO PRINCIPAL */}
    <main className="bg-white rounded-3xl shadow-xl border border-slate-100 p-8 min-h-[500px]">
      
      {currentSection === 'intro' && (
        <div className="space-y-6">
          <h2 className="text-3xl font-black text-slate-800">Conceptos Básicos</h2>
          <div className="p-6 bg-blue-50 rounded-2xl border-l-8 border-blue-800">
            <p className="text-lg text-blue-900 leading-relaxed font-medium">
              Una <strong>expresión algebraica</strong> combina números y letras unidos por operaciones. 
              En este curso aprenderás a dominarlas como un lenguaje real.
            </p>
          </div>
          <div className="grid md:grid-cols-2 gap-4 mt-8">
            <div className="p-5 border-2 border-slate-100 rounded-2xl hover:border-blue-200 transition-colors">
              <span className="block text-blue-800 font-black mb-2">Ejemplo 1</span>
              <p className="text-2xl font-mono font-bold">3x + 5y</p>
            </div>
            <div className="p-5 border-2 border-slate-100 rounded-2xl hover:border-blue-200 transition-colors">
              <span className="block text-blue-800 font-black mb-2">Ejemplo 2</span>
              <p className="text-2xl font-mono font-bold">x² - 9</p>
            </div>
          </div>
        </div>
      )}

      {currentSection === 'components' && (
        <div className="space-y-8">
          <h2 className="text-3xl font-black text-slate-800">Partes del Término</h2>
          <div className="bg-slate-900 rounded-3xl p-10 text-center">
             <div className="text-6xl font-mono font-black tracking-tighter">
               <span className="text-red-500">-5</span>
               <span className="text-white">x</span>
               <sup className="text-emerald-400 text-3xl">3</sup>
             </div>
             <div className="flex justify-center gap-4 mt-8 text-xs font-black uppercase tracking-widest">
                <span className="bg-red-500/20 text-red-400 px-3 py-1 rounded">Coeficiente</span>
                <span className="bg-white/20 text-white px-3 py-1 rounded">Literal/Base</span>
                <span className="bg-emerald-500/20 text-emerald-400 px-3 py-1 rounded">Exponente</span>
             </div>
          </div>
          
          <div className="space-y-4">
            {identificationQuiz.map((q, idx) => (
              <div key={idx} className="p-6 bg-slate-50 rounded-2xl border border-slate-200">
                <p className="font-black text-slate-700 mb-4">{q.question}</p>
                <div className="grid grid-cols-2 md:grid-cols-4 gap-3">
                  {q.options.map((opt, oIdx) => (
                    <button
                      key={oIdx}
                      onClick={() => handleQuizAnswer('identification', idx, oIdx)}
                      className={`p-3 rounded-xl font-bold transition-all border-2 ${
                        answered[`identification-${idx}`]?.selected === oIdx
                          ? (q.correct === oIdx ? 'bg-emerald-500 border-emerald-500 text-white' : 'bg-red-500 border-red-500 text-white')
                          : 'bg-white border-slate-100 hover:border-blue-300'
                      }`}
                    >
                      {opt}
                    </button>
                  ))}
                </div>
              </div>
            ))}
          </div>
        </div>
      )}

      {currentSection === 'similar' && (
        <div className="space-y-6">
          <h2 className="text-3xl font-black text-slate-800">Clasificación de Términos</h2>
          <p className="text-slate-500 font-medium">Arrastra cada término a su grupo correcto para sumarlos luego.</p>
          
          <div className="flex flex-wrap gap-3 p-6 bg-slate-100 rounded-2xl border-2 border-dashed border-slate-300">
            {termItems.filter(item => !Object.values(droppedItems).flat().find(d => d.id === item.id)).map(item => (
              <div
                key={item.id}
                draggable
                onDragStart={() => handleDragStart(item)}
                className="px-6 py-3 bg-white shadow-lg rounded-xl cursor-move font-black text-blue-700 border border-blue-100 active:scale-90 transition-transform"
              >
                {item.term}
              </div>
            ))}
          </div>

          <div className="grid md:grid-cols-2 gap-6 mt-8">
            {['A', 'B'].map(zone => (
              <div
                key={zone}
                onDragOver={(e) => e.preventDefault()}
                onDrop={() => handleDrop(zone)}
                className={`min-h-[150px] p-6 rounded-3xl border-4 border-dashed transition-colors ${
                  zone === 'A' ? 'bg-emerald-50 border-emerald-200' : 'bg-purple-50 border-purple-200'
                }`}
              >
                <h3 className="font-black text-slate-700 mb-4 uppercase text-xs tracking-widest">
                  {zone === 'A' ? 'Grupo x²' : 'Grupo xy'}
                </h3>
                <div className="flex flex-wrap gap-2">
                  {droppedItems[zone].map(item => (
                    <span key={item.id} className="px-3 py-1 bg-white rounded-lg font-bold shadow-sm">{item.term}</span>
                  ))}
                </div>
              </div>
            ))}
          </div>
        </div>
      )}

      {currentSection === 'calculator' && (
        <div className="space-y-6">
          <h2 className="text-3xl font-black text-slate-800">Valor Numérico</h2>
          <div className="p-8 bg-blue-900 rounded-3xl text-white text-center">
            <p className="text-sm font-black text-blue-300 uppercase mb-2">Expresión a evaluar</p>
            <p className="text-5xl font-mono font-black">2x² + 3xy - z</p>
          </div>

          <div className="grid grid-cols-3 gap-4">
            {['x', 'y', 'z'].map(v => (
              <div key={v}>
                <label className="block text-xs font-black text-slate-400 uppercase mb-2">Valor {v}</label>
                <input
                  type="number"
                  value={calculatorInputs[v]}
                  onChange={(e) => setCalculatorInputs({...calculatorInputs, [v]: e.target.value})}
                  className="w-full p-4 bg-slate-50 border-2 border-slate-100 rounded-2xl focus:border-blue-500 focus:outline-none font-bold text-xl"
                  placeholder="0"
                />
              </div>
            ))}
          </div>

          <button 
            onClick={calculateValue}
            className="w-full bg-blue-800 text-white py-5 rounded-2xl font-black text-xl hover:bg-blue-900 transition-all shadow-xl shadow-blue-100"
          >
            CALCULAR RESULTADO
          </button>

          {calculatorResult !== null && (
            <div className="p-6 bg-emerald-50 rounded-2xl border-2 border-emerald-200 text-center animate-bounce">
              <span className="block text-emerald-600 font-black text-xs uppercase mb-1">Resultado</span>
              <span className="text-5xl font-black text-emerald-700">{calculatorResult}</span>
            </div>
          )}
        </div>
      )}

      {currentSection === 'quiz' && (
        <div className="text-center py-12">
           <Trophy className="mx-auto text-amber-400 mb-6" size={80}/>
           <h2 className="text-4xl font-black text-slate-800 mb-2">¡Módulo Completado!</h2>
           <p className="text-slate-500 font-bold mb-10">Has demostrado dominio en Expresiones Algebraicas.</p>
           
           <div className="max-w-xs mx-auto p-8 bg-slate-50 rounded-3xl border-2 border-slate-100">
              <div className="text-sm font-black text-slate-400 uppercase mb-1">Tu puntaje es de</div>
              <div className="text-6xl font-black text-blue-800">{score}</div>
              <div className="mt-6 flex justify-center gap-1">
                {[1,2,3,4,5].map(i => (
                  <Star key={i} className={i <= score ? "text-amber-400 fill-amber-400" : "text-slate-200"} size={20}/>
                ))}
              </div>
           </div>
        </div>
      )}

    </main>

    <footer className="mt-8 flex justify-center items-center gap-2 text-slate-400 font-black text-xs uppercase tracking-widest">
       <Award size={16}/> Educación de alto rendimiento
    </footer>
  </div>
</div>
