import 'package:flutter/material.dart';
import 'package:intl/intl.dart';
import 'package:fl_chart/fl_chart.dart';

void main() {
  runApp(const MyApp());
}

class Peso {

  final double peso;
  final DateTime fecha;

  Peso(this.peso, this.fecha);
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Evolución de Peso del Bebé',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const HomePage(),
    );
  }
}

class HomePage extends StatefulWidget {
  const HomePage({super.key});

  @override
  State<HomePage
> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  final List<Peso> _pesos = [];
  final TextEditingController _pesoController = TextEditingController();

  void _agregarPeso() {
    double? peso = double.tryParse(_pesoController.text);
    if (peso != null) {
      setState(() {
        _pesos.add(Peso(peso, DateTime.now()));
        _pesoController.clear();
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Evolución de Peso del Bebé'),
      ),
      body: Column(
        children: [
          Padding(
            padding: const EdgeInsets.all(16.0),
            child: TextField(
              controller: _pesoController,
              keyboardType: TextInputType.number,
              decoration: const InputDecoration(
                labelText: 'Peso (kg)',
              ),
            ),
          ),
          ElevatedButton(
            onPressed: _agregarPeso,
            child: const Text('Agregar Peso'),
          ),
          const SizedBox(height: 16),
          const Text(
            'Evolución del Peso',
            style: TextStyle(
              fontSize: 20,
              fontWeight: FontWeight.bold,
            ),
          ),
          Expanded(
            child: Padding(
              padding: const EdgeInsets.all(8.0),
              child: _pesos.isEmpty
                  ? const Center(child: Text('Aún no hay datos'))
                  : LineChart(
                      LineChartData(
                        minX: 0,
                        maxX: _pesos.length.toDouble() - 1,
                        minY: _pesos.map((p) => p.peso).reduce((a, b) => a < b ? a : b) - 1,
                        maxY: _pesos.map((p) => p.peso).reduce((a, b) => a > b ? a : b) + 1,
                        lineBarsData: [
                          LineChartBarData(
                            spots: _pesos.asMap().entries.map((entry) {
                              return FlSpot(entry.key.toDouble(), entry.value.peso);
                            }).toList(),
                            isCurved: true,
                            color: Colors.blue,
                            dotData: const FlDotData(show: false),
                            belowBarData: BarAreaData(
                              show: true,
                              color: Colors.blue.withOpacity(0.3),
                            ),
                          ),
                        ],
                        titlesData: FlTitlesData(
                          bottomTitles: AxisTitles(
                            sideTitles: SideTitles(
                              showTitles: true,
                              getTitlesWidget: (value, meta) {
                                final index = value.toInt();
                                if (index >= 0 && index < _pesos.length) {
                                  final fecha = _pesos[index].fecha;
                                  return Text(DateFormat('dd/MM').format(fecha));
                                }
                                return const Text('');
                              },
                            ),
                          ),
                          leftTitles: const AxisTitles(
                            sideTitles: SideTitles(
                              showTitles: true,
                            ),
                          ),
                        ),
                      ),
                    ),
            ),
          ),
        ],
      ),
    );
  }
}
