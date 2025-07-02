# Melhores-investimentos
// App.jsx
import React, { useEffect, useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";

export default function App() {
  const [investimentos, setInvestimentos] = useState([]);
  const [filtro, setFiltro] = useState("Todos");
  const [ordenarPor, setOrdenarPor] = useState("rentabilidade");

  useEffect(() => {
    async function fetchData() {
      const lista = [];

      // Bitcoin (CoinGecko)
      const btc = await fetch(
        "https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=brl&include_24hr_change=true"
      ).then((res) => res.json());

      lista.push({
        nome: "Bitcoin",
        tipo: "Cripto",
        risco: "Alto",
        valor: btc.bitcoin.brl,
        rentabilidade: btc.bitcoin.brl_24h_change,
      });

      // Ações (exemplo fictício)
      lista.push({
        nome: "PETR4",
        tipo: "Ações",
        risco: "Médio",
        valor: 38.7,
        rentabilidade: 1.25,
      });

      // Renda Fixa
      lista.push({
        nome: "CDB Liquidez Diária (PagBank)",
        tipo: "Renda Fixa",
        risco: "Baixo",
        valor: 100,
        rentabilidade: 0.47,
      });

      lista.push({
        nome: "Tesouro Selic 2027",
        tipo: "Renda Fixa",
        risco: "Baixo",
        valor: 112.35,
        rentabilidade: 0.10,
      });

      // ETFs
      lista.push({
        nome: "BOVA11",
        tipo: "ETF",
        risco: "Médio",
        valor: 126.8,
        rentabilidade: 0.80,
      });

      lista.push({
        nome: "IVVB11",
        tipo: "ETF",
        risco: "Médio",
        valor: 312.4,
        rentabilidade: 1.20,
      });

      setInvestimentos(lista);
    }

    fetchData();
  }, []);

  const tipos = ["Todos", "Cripto", "Ações", "Renda Fixa", "ETF"];
  const riscos = ["Todos", "Alto", "Médio", "Baixo"];

  const filtrados =
    filtro === "Todos"
      ? investimentos
      : investimentos.filter((i) => i.tipo === filtro);

  const ordenados = [...filtrados].sort((a, b) => {
    return ordenarPor === "rentabilidade"
      ? b.rentabilidade - a.rentabilidade
      : a.valor - b.valor;
  });

  return (
    <main className="p-6 max-w-6xl mx-auto">
      <h1 className="text-3xl font-bold mb-4">💰 Melhores Investimentos do Dia</h1>

      <div className="flex flex-wrap gap-2 mb-4">
        {tipos.map((tipo) => (
          <Button key={tipo} onClick={() => setFiltro(tipo)}>
            {tipo}
          </Button>
        ))}
        <Button onClick={() => setOrdenarPor("rentabilidade")}>
          Ordenar por Rentabilidade
        </Button>
        <Button onClick={() => setOrdenarPor("valor")}>
          Ordenar por Valor
        </Button>
      </div>

      <div className="grid md:grid-cols-2 lg:grid-cols-3 gap-4">
        {ordenados.map((inv, idx) => (
          <Card key={idx}>
            <CardContent className="p-4">
              <h2 className="text-xl font-semibold">{inv.nome}</h2>
              <p className="text-gray-500">{inv.tipo} • Risco: {inv.risco}</p>
              <p className="text-green-600 font-medium">
                Valor: R$ {inv.valor.toLocaleString(undefined, { minimumFractionDigits: 2 })}
              </p>
              <p className="text-sm text-gray-600">
                Rentabilidade: {inv.rentabilidade.toFixed(2)}%
              </p>
            </CardContent>
          </Card>
        ))}
      </div>
    </main>
  );
}
