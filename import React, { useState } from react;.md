import React, { useState } from "react";  
  
export default function DashboardPlanificacion() {  
  const designaciones = [  
    "Ayudante",  
    "Operador C",  
    "Operador B",  
    "Operador A",  
    "Supervisor",  
    "Verificador",  
  ];  
  
  const horariosDisponibles = [  
    "06:00 AM - 14:00 PM",  
    "06:00 AM - 18:00 PM",  
    "14:00 PM - 21:00 PM",  
    "16:00 PM - 22:00 PM",  
    "18:00 PM - 06:00 AM",  
    "Libre",  
  ];  
  
  const dataInicial = [  
    {  
      nombre: "Grupo A",  
      maquinas: [  
        {  
          nombre: "SEN FUNG",  
          horarios: {  
            lv: "06:00 AM - 14:00 PM",  
            sabado: "06:00 AM - 14:00 PM",  
            domingo: "Libre",  
          },  
          operadores: [  
            { nombre: "Willian Pacheco", puesto: "Operador B" },  
            { nombre: "Marchena", puesto: "Operador C" },  
          ],  
        },  
  
        {  
          nombre: "Slitter Guida",  
          horarios: {  
            lv: "06:00 AM - 14:00 PM",  
            sabado: "06:00 AM - 14:00 PM",  
            domingo: "Libre",  
          },  
          operadores: [  
            { nombre: "German Avalos", puesto: "Operador A" },  
            { nombre: "Jeffrey Rivas", puesto: "Ayudante" },  
          ],  
        },  
      ],  
    },  
  
    {  
      nombre: "Grupo B",  
      maquinas: [  
        {  
          nombre: "Molino 1274",  
          horarios: {  
            lv: "14:00 PM - 21:00 PM",  
            sabado: "14:00 PM - 21:00 PM",  
            domingo: "Libre",  
          },  
          operadores: [  
            { nombre: "Gerson Perez", puesto: "Operador A" },  
          ],  
        },  
  
        {  
          nombre: "Molino 633",  
          horarios: {  
            lv: "14:00 PM - 21:00 PM",  
            sabado: "14:00 PM - 21:00 PM",  
            domingo: "Libre",  
          },  
          operadores: [  
            { nombre: "David Garita", puesto: "Operador A" },  
          ],  
        },  
      ],  
    },  
  ];  
  
  const [grupos, setGrupos] = useState(dataInicial);  
  
  const [vistaResumen, setVistaResumen] =  
    useState(false);  
  
  const [nuevaMaquina, setNuevaMaquina] =  
    useState("");  
  
  const agregarOperador = (  
    grupoIndex,  
    maquinaIndex  
  ) => {  
    const copia = [...grupos];  
  
    copia[grupoIndex].maquinas[  
      maquinaIndex  
    ].operadores.push({  
      nombre: "",  
      puesto: "Ayudante",  
    });  
  
    setGrupos(copia);  
  };  
  
  const eliminarOperador = (  
    grupoIndex,  
    maquinaIndex,  
    operadorIndex  
  ) => {  
    const copia = [...grupos];  
  
    copia[grupoIndex].maquinas[  
      maquinaIndex  
    ].operadores.splice(operadorIndex, 1);  
  
    setGrupos(copia);  
  };  
  
  const agregarMaquina = (grupoIndex) => {  
    if (!nuevaMaquina) return;  
  
    const copia = [...grupos];  
  
    copia[grupoIndex].maquinas.push({  
      nombre: nuevaMaquina,  
      horarios: {  
        lv: "06:00 AM - 14:00 PM",  
        sabado: "06:00 AM - 14:00 PM",  
        domingo: "Libre",  
      },  
      operadores: [],  
    });  
  
    setGrupos(copia);  
  
    setNuevaMaquina("");  
  };  
  
  const eliminarMaquina = (  
    grupoIndex,  
    maquinaIndex  
  ) => {  
    const copia = [...grupos];  
  
    copia[grupoIndex].maquinas.splice(  
      maquinaIndex,  
      1  
    );  
  
    setGrupos(copia);  
  };  
  
  const contarPuestos = (grupo) => {  
    const conteo = {  
      "Operador A": 0,  
      "Operador B": 0,  
      "Operador C": 0,  
      Ayudante: 0,  
      Supervisor: 0,  
      Verificador: 0,  
    };  
  
    grupo.maquinas.forEach((maq) => {  
      maq.operadores.forEach((op) => {  
        if (conteo[op.puesto] !== undefined) {  
          conteo[op.puesto]++;  
        }  
      });  
    });  
  
    return conteo;  
  };  
  
  return (  
    <div className="min-h-screen bg-gray-200 p-6">  
      <div className="mb-8 rounded-3xl bg-red-700 p-6 text-white shadow-2xl">  
        <h1 className="text-4xl font-bold">  
          PLANIFICACIÓN TUBOACERO  
        </h1>  
  
        <p className="mt-2 text-lg">  
          Dashboard Operativo Industrial  
        </p>  
      </div>  
  
      <div className="mb-6 flex gap-4">  
        <button  
          onClick={() => setVistaResumen(false)}  
          className={`rounded-xl px-5 py-3 font-bold ${  
            !vistaResumen  
              ? "bg-red-700 text-white"  
              : "bg-white"  
          }`}  
        >  
          Vista Editable  
        </button>  
  
        <button  
          onClick={() => setVistaResumen(true)}  
          className={`rounded-xl px-5 py-3 font-bold ${  
            vistaResumen  
              ? "bg-red-700 text-white"  
              : "bg-white"  
          }`}  
        >  
          Vista Resumen  
        </button>  
      </div>  
  
      <div className="space-y-12">  
        {grupos.map((grupo, grupoIndex) => {  
          const conteo = contarPuestos(grupo);  
  
          const total =  
            grupo.maquinas.reduce(  
              (acc, maq) =>  
                acc + maq.operadores.length,  
              0  
            );  
  
          return (  
            <div key={grupoIndex}>  
              <div className="mb-6 rounded-3xl bg-red-700 p-5 text-white shadow-2xl">  
                <div className="flex flex-col gap-4 md:flex-row md:items-center md:justify-between">  
                  <div>  
                    <h2 className="text-3xl font-bold">  
                      {grupo.nombre}  
                    </h2>  
  
                    <p className="mt-2 text-lg">  
                      Total Operadores: {total}  
                    </p>  
                  </div>  
  
                  <div className="grid grid-cols-2 gap-2 rounded-2xl bg-white/10 p-4 text-sm">  
                    <div>  
                      Operador A:  
                      {conteo["Operador A"]}  
                    </div>  
  
                    <div>  
                      Operador B:  
                      {conteo["Operador B"]}  
                    </div>  
  
                    <div>  
                      Operador C:  
                      {conteo["Operador C"]}  
                    </div>  
  
                    <div>  
                      Ayudantes:  
                      {conteo["Ayudante"]}  
                    </div>  
  
                    <div>  
                      Supervisores:  
                      {conteo["Supervisor"]}  
                    </div>  
  
                    <div>  
                      Verificadores:  
                      {conteo["Verificador"]}  
                    </div>  
                  </div>  
                </div>  
              </div>  
  
              {!vistaResumen && (  
                <div className="mb-6 rounded-2xl bg-white p-4 shadow-xl">  
                  <div className="flex flex-col gap-3 md:flex-row">  
                    <input  
                      value={nuevaMaquina}  
                      onChange={(e) =>  
                        setNuevaMaquina(  
                          e.target.value  
                        )  
                      }  
                      placeholder="Nueva máquina"  
                      className="flex-1 rounded-xl border border-gray-300 p-3"  
                    />  
  
                    <button  
                      onClick={() =>  
                        agregarMaquina(  
                          grupoIndex  
                        )  
                      }  
                      className="rounded-xl bg-blue-600 px-5 py-3 font-bold text-white hover:bg-blue-700"  
                    >  
                      + Agregar Máquina  
                    </button>  
                  </div>  
                </div>  
              )}  
  
              <div className="grid grid-cols-1 gap-6 md:grid-cols-2 xl:grid-cols-3">  
                {grupo.maquinas.map(  
                  (maq, maqIndex) => (  
                    <div  
                      key={maqIndex}  
                      className="rounded-3xl border-2 border-black bg-white shadow-2xl"  
                    >  
                      <div className="flex items-center justify-between rounded-t-3xl bg-red-700 p-4 text-white">  
                        <h3 className="text-xl font-bold">  
                          {maq.nombre}  
                        </h3>  
  
                        {!vistaResumen && (  
                          <button  
                            onClick={() =>  
                              eliminarMaquina(  
                                grupoIndex,  
                                maqIndex  
                              )  
                            }  
                            className="rounded-lg bg-red-900 px-3 py-1 text-sm font-bold text-white"  
                          >  
                            X  
                          </button>  
                        )}  
                      </div>  
  
                      <div className="p-4">  
                        <div className="mb-4 overflow-hidden rounded-xl border border-gray-300">  
                          <table className="w-full border-collapse">  
                            <thead>  
                              <tr className="bg-gray-200">  
                                <th className="border p-2 text-left">  
                                  Nombre  
                                </th>  
  
                                <th className="border p-2 text-left">  
                                  Puesto  
                                </th>  
  
                                {!vistaResumen && (  
                                  <th className="border p-2 text-center">  
                                    Acción  
                                  </th>  
                                )}  
                              </tr>  
                            </thead>  
  
                            <tbody>  
                              {maq.operadores.map(  
                                (  
                                  op,  
                                  opIndex  
                                ) => (  
                                  <tr  
                                    key={  
                                      opIndex  
                                    }  
                                    className="hover:bg-gray-50"  
                                  >  
                                    <td className="border p-2">  
                                      {vistaResumen ? (  
                                        <span>  
                                          {  
                                            op.nombre  
                                          }  
                                        </span>  
                                      ) : (  
                                        <input  
                                          value={  
                                            op.nombre  
                                          }  
                                          onChange={(  
                                            e  
                                          ) => {  
                                            const copia =  
                                              [  
                                                ...grupos,  
                                              ];  
  
                                            copia[  
                                              grupoIndex  
                                            ]  
                                              .maquinas[  
                                              maqIndex  
                                            ]  
                                              .operadores[  
                                              opIndex  
                                            ].nombre =  
                                              e.target.value;  
  
                                            setGrupos(  
                                              copia  
                                            );  
                                          }}  
                                          placeholder="Nombre operador"  
                                          className="w-full rounded border border-gray-300 p-1"  
                                        />  
                                      )}  
                                    </td>  
  
                                    <td className="border p-2">  
                                      {vistaResumen ? (  
                                        <span>  
                                          {  
                                            op.puesto  
                                          }  
                                        </span>  
                                      ) : (  
                                        <select  
                                          value={  
                                            op.puesto  
                                          }  
                                          onChange={(  
                                            e  
                                          ) => {  
                                            const copia =  
                                              [  
                                                ...grupos,  
                                              ];  
  
                                            copia[  
                                              grupoIndex  
                                            ]  
                                              .maquinas[  
                                              maqIndex  
                                            ]  
                                              .operadores[  
                                              opIndex  
                                            ].puesto =  
                                              e.target.value;  
  
                                            setGrupos(  
                                              copia  
                                            );  
                                          }}  
                                          className="w-full rounded border border-gray-300 p-1"  
                                        >  
                                          {designaciones.map(  
                                            (  
                                              d,  
                                              i  
                                            ) => (  
                                              <option  
                                                key={  
                                                  i  
                                                }  
                                              >  
                                                {  
                                                  d  
                                                }  
                                              </option>  
                                            )  
                                          )}  
                                        </select>  
                                      )}  
                                    </td>  
  
                                    {!vistaResumen && (  
                                      <td className="border p-2 text-center">  
                                        <button  
                                          onClick={() =>  
                                            eliminarOperador(  
                                              grupoIndex,  
                                              maqIndex,  
                                              opIndex  
                                            )  
                                          }  
                                          className="rounded-lg bg-red-600 px-2 py-1 text-sm font-bold text-white"  
                                        >  
                                          X  
                                        </button>  
                                      </td>  
                                    )}  
                                  </tr>  
                                )  
                              )}  
                            </tbody>  
                          </table>  
                        </div>  
  
                        {!vistaResumen && (  
                          <>  
                            <div className="mb-4 rounded-2xl bg-gray-100 p-4">  
                              <p className="mb-3 font-bold text-red-700">  
                                Horarios  
                              </p>  
  
                              <div className="space-y-3">  
                                <select className="w-full rounded-lg border border-gray-300 p-2">  
                                  {horariosDisponibles.map(  
                                    (  
                                      h,  
                                      i  
                                    ) => (  
                                      <option  
                                        key={  
                                          i  
                                        }  
                                      >  
                                        {  
                                          h  
                                        }  
                                      </option>  
                                    )  
                                  )}  
                                </select>  
  
                                <select className="w-full rounded-lg border border-gray-300 p-2">  
                                  {horariosDisponibles.map(  
                                    (  
                                      h,  
                                      i  
                                    ) => (  
                                      <option  
                                        key={  
                                          i  
                                        }  
                                      >  
                                        {  
                                          h  
                                        }  
                                      </option>  
                                    )  
                                  )}  
                                </select>  
  
                                <select className="w-full rounded-lg border border-gray-300 p-2">  
                                  {horariosDisponibles.map(  
                                    (  
                                      h,  
                                      i  
                                    ) => (  
                                      <option  
                                        key={  
                                          i  
                                        }  
                                      >  
                                        {  
                                          h  
                                        }  
                                      </option>  
                                    )  
                                  )}  
                                </select>  
                              </div>  
                            </div>  
  
                            <button  
                              onClick={() =>  
                                agregarOperador(  
                                  grupoIndex,  
                                  maqIndex  
                                )  
                              }  
                              className="w-full rounded-2xl bg-green-600 p-3 font-bold text-white hover:bg-green-700"  
                            >  
                              + Agregar  
                              Operador  
                            </button>  
                          </>  
                        )}  
                      </div>  
                    </div>  
                  )  
                )}  
              </div>  
            </div>  
          );  
        })}  
      </div>  
    </div>  
  );  
}  
