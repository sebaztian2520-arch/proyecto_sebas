import 'package:flutter/material.dart';

void main() {
  runApp(const HojaDeViajesApp());
}

class HojaDeViajesApp extends StatelessWidget {
  const HojaDeViajesApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Hoja de Viajes',
      debugShowCheckedModeBanner: false,
      theme: ThemeData.dark().copyWith(
        scaffoldBackgroundColor: const Color(0xFF0F172A),
        inputDecorationTheme: InputDecorationTheme(
          filled: true,
          fillColor: const Color(0xFF1F2937),
          border: OutlineInputBorder(
            borderRadius: BorderRadius.circular(8),
          ),
          enabledBorder: OutlineInputBorder(
            borderRadius: BorderRadius.circular(8),
            borderSide: const BorderSide(color: Color(0xFF374151)),
          ),
          labelStyle: const TextStyle(color: Color(0xFFCBD5F5)),
          hintStyle: const TextStyle(color: Color(0xFF9CA3AF)),
          contentPadding:
              const EdgeInsets.symmetric(horizontal: 10, vertical: 8),
        ),
      ),
      home: const DatosGeneralesPage(),
    );
  }
}

class DatosGenerales {
  final String fecha;
  final String nombre;
  final String zona;
  final String turno;
  final String horometro;
  final String combustible;
  final String kilometraje;
  final String codigoUnidad;

  DatosGenerales({
    required this.fecha,
    required this.nombre,
    required this.zona,
    required this.turno,
    required this.horometro,
    required this.combustible,
    required this.kilometraje,
    required this.codigoUnidad,
  });
}

class CodigoActividad {
  final String codigo;
  final String descripcion;

  CodigoActividad({required this.codigo, required this.descripcion});
}

final List<CodigoActividad> codigosActividad = [
  CodigoActividad(codigo: "1", descripcion: "ACARREO"),
  CodigoActividad(codigo: "2", descripcion: "ACARREO DE RIPIOS, ARENA"),
  CodigoActividad(codigo: "3", descripcion: "DEMORA EN SCOOP / ESPERANDO SCOOP"),
  CodigoActividad(codigo: "4", descripcion: "DEMORA DE CARGUÍO / ESPERANDO TURNO"),
  CodigoActividad(codigo: "5", descripcion: "EQUIPO DE CARGUÍO INOPERATIVO"),
  CodigoActividad(codigo: "6", descripcion: "CAMBIO A OTRA LABOR"),
  CodigoActividad(codigo: "7", descripcion: "TRÁFICO EN VÍA"),
  CodigoActividad(codigo: "8", descripcion: "SALIDA SIN CARGA DE LA LABOR"),
  CodigoActividad(codigo: "9", descripcion: "DEMORA EN LA DESCARGA / ESPERANDO TURNO"),
  CodigoActividad(
      codigo: "10", descripcion: "DEMORA EN DESCARGAR POR COND. LABORES"),
  CodigoActividad(codigo: "11", descripcion: "DEMORAS OPERATIVAS POR LA EMPRESA"),
  CodigoActividad(codigo: "12", descripcion: "ENTREGA DE VOLQUETE AL RELEVO"),
  CodigoActividad(codigo: "13", descripcion: "CHEQUEO DEL EQUIPO / CHECK LIST"),
  CodigoActividad(codigo: "14", descripcion: "REPARTO DE GUARDIA"),
  CodigoActividad(codigo: "15", descripcion: "TRASLADO A LA LABOR"),
  CodigoActividad(codigo: "16", descripcion: "TRASLADO A TALLER / GRIFO"),
  CodigoActividad(codigo: "17", descripcion: "LAVADO DE EQUIPO"),
  CodigoActividad(codigo: "18", descripcion: "STAND BY / CAMBIO DE VOLQUETE"),
  CodigoActividad(codigo: "19", descripcion: "ABASTECIMIENTO DE COMBUSTIBLE"),
  CodigoActividad(codigo: "20", descripcion: "PARADOS POR PROBLEMAS DE LLANTAS"),
  CodigoActividad(codigo: "21", descripcion: "PARADO POR FALLA MECÁNICA"),
  CodigoActividad(codigo: "22", descripcion: "PARADO POR FALLA ELÉCTRICA"),
  CodigoActividad(codigo: "23", descripcion: "MANTENIMIENTO PREVENTIVO"),
  CodigoActividad(codigo: "24", descripcion: "MANTENIMIENTO PROGRAMADO"),
  CodigoActividad(codigo: "25", descripcion: "MANTENIMIENTO CORRECTIVO"),
  CodigoActividad(codigo: "26", descripcion: "DAÑO AL EQUIPO"),
  CodigoActividad(codigo: "27", descripcion: "ALMUERZO O DESCANSO"),
];

class Viaje {
  final String id;
  final String nroViaje;
  final String horaLlegada;
  final String inicioCarga;
  final String finCarga;
  final String horaDescarga;
  final String carguioKm;
  final String descargaKm;
  final String codigoActividad;
  final String nivel;
  final String labor;
  final String destino;
  final String material;
  final String equipoCarguioObs;

  Viaje({
    required this.id,
    required this.nroViaje,
    required this.horaLlegada,
    required this.inicioCarga,
    required this.finCarga,
    required this.horaDescarga,
    required this.carguioKm,
    required this.descargaKm,
    required this.codigoActividad,
    required this.nivel,
    required this.labor,
    required this.destino,
    required this.material,
    required this.equipoCarguioObs,
  });
}

class DatosGeneralesPage extends StatefulWidget {
  const DatosGeneralesPage({super.key});

  @override
  State<DatosGeneralesPage> createState() => _DatosGeneralesPageState();
}

class _DatosGeneralesPageState extends State<DatosGeneralesPage> {
  final _formKey = GlobalKey<FormState>();

  final TextEditingController fechaCtrl = TextEditingController();
  final TextEditingController nombreCtrl = TextEditingController();
  final TextEditingController zonaCtrl = TextEditingController();
  final TextEditingController turnoCtrl = TextEditingController();
  final TextEditingController horometroCtrl = TextEditingController();
  final TextEditingController combustibleCtrl = TextEditingController();
  final TextEditingController kilometrajeCtrl = TextEditingController();
  final TextEditingController codigoUnidadCtrl = TextEditingController();

  String? _validateNotEmpty(String? value) {
    if (value == null || value.trim().isEmpty) {
      return 'Campo obligatorio';
    }
    return null;
  }

  String? _validateNumero(String? value) {
    if (value == null || value.trim().isEmpty) {
      return 'Campo obligatorio';
    }
    if (double.tryParse(value.replaceAll(',', '.')) == null) {
      return 'Solo números';
    }
    return null;
  }

  void _continuar() {
    if (_formKey.currentState?.validate() ?? false) {
      final datos = DatosGenerales(
        fecha: fechaCtrl.text.trim(),
        nombre: nombreCtrl.text.trim(),
        zona: zonaCtrl.text.trim(),
        turno: turnoCtrl.text.trim(),
        horometro: horometroCtrl.text.trim(),
        combustible: combustibleCtrl.text.trim(),
        kilometraje: kilometrajeCtrl.text.trim(),
        codigoUnidad: codigoUnidadCtrl.text.trim(),
      );

      Navigator.of(context).pushReplacement(
        MaterialPageRoute(
          builder: (_) => HojaDeViajesPage(datosGenerales: datos),
        ),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Datos generales'),
        centerTitle: true,
        backgroundColor: const Color(0xFF020617),
      ),
      body: SafeArea(
        child: SingleChildScrollView(
          padding: const EdgeInsets.all(16),
          child: Form(
            key: _formKey,
            child: Column(
              children: [
                const Text(
                  'HOJA DE VIAJES',
                  style: TextStyle(
                    fontSize: 24,
                    fontWeight: FontWeight.w700,
                    color: Color(0xFFE5E7EB),
                  ),
                ),
                const SizedBox(height: 16),
                _campo(
                  label: 'Fecha',
                  controller: fechaCtrl,
                  hint: 'dd/mm/aaaa',
                  validator: _validateNotEmpty,
                ),
                _campo(
                  label: 'Nombre (operador)',
                  controller: nombreCtrl,
                  validator: _validateNotEmpty,
                ),
                _campo(
                  label: 'Zona',
                  controller: zonaCtrl,
                  validator: _validateNotEmpty,
                ),
                _campo(
                  label: 'Turno',
                  controller: turnoCtrl,
                  hint: 'Día / Noche',
                  validator: _validateNotEmpty,
                ),
                _campo(
                  label: 'Horómetro',
                  controller: horometroCtrl,
                  hint: 'Ej. 1234.5',
                  keyboardType:
                      const TextInputType.numberWithOptions(decimal: true),
                  validator: _validateNumero,
                ),
                _campo(
                  label: 'Combustible',
                  controller: combustibleCtrl,
                  hint: 'Litros',
                  keyboardType:
                      const TextInputType.numberWithOptions(decimal: true),
                  validator: _validateNumero,
                ),
                _campo(
                  label: 'Kilometraje',
                  controller: kilometrajeCtrl,
                  hint: 'km inicio',
                  keyboardType:
                      const TextInputType.numberWithOptions(decimal: true),
                  validator: _validateNumero,
                ),
                _campo(
                  label: 'Código de unidad',
                  controller: codigoUnidadCtrl,
                  hint: 'VOL_01, CAM_02, etc.',
                  validator: _validateNotEmpty,
                ),
                const SizedBox(height: 20),
                SizedBox(
                  width: double.infinity,
                  child: ElevatedButton(
                    onPressed: _continuar,
                    style: ElevatedButton.styleFrom(
                      backgroundColor: const Color(0xFF22C55E),
                      foregroundColor: const Color(0xFF052E16),
                      padding: const EdgeInsets.symmetric(vertical: 12),
                      shape: RoundedRectangleBorder(
                        borderRadius: BorderRadius.circular(10),
                      ),
                    ),
                    child: const Text(
                      'Continuar con registros de viajes',
                      style: TextStyle(
                        fontWeight: FontWeight.w700,
                        fontSize: 14,
                      ),
                    ),
                  ),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }

  Widget _campo({
    required String label,
    required TextEditingController controller,
    String? hint,
    TextInputType keyboardType = TextInputType.text,
    String? Function(String?)? validator,
  }) {
    return Padding(
      padding: const EdgeInsets.only(bottom: 10),
      child: TextFormField(
        controller: controller,
        keyboardType: keyboardType,
        validator: validator,
        style: const TextStyle(fontSize: 13, color: Color(0xFFF9FAFB)),
        decoration: InputDecoration(
          labelText: label,
          hintText: hint,
        ),
      ),
    );
  }
}

class HojaDeViajesPage extends StatefulWidget {
  final DatosGenerales datosGenerales;

  const HojaDeViajesPage({super.key, required this.datosGenerales});

  @override
  State<HojaDeViajesPage> createState() => _HojaDeViajesPageState();
}

class _HojaDeViajesPageState extends State<HojaDeViajesPage> {
  final TextEditingController nroViajeCtrl = TextEditingController();
  final TextEditingController horaLlegadaCtrl = TextEditingController();
  final TextEditingController inicioCargaCtrl = TextEditingController();
  final TextEditingController finCargaCtrl = TextEditingController();
  final TextEditingController horaDescargaCtrl = TextEditingController();
  final TextEditingController carguioKmCtrl = TextEditingController();
  final TextEditingController descargaKmCtrl = TextEditingController();
  final TextEditingController nivelCtrl = TextEditingController();
  final TextEditingController laborCtrl = TextEditingController();
  final TextEditingController destinoCtrl = TextEditingController();
  final TextEditingController materialCtrl = TextEditingController();
  final TextEditingController equipoCarguioObsCtrl = TextEditingController();

  String? codigoSeleccionado;

  final List<Viaje> viajes = [];

  @override
  void dispose() {
    nroViajeCtrl.dispose();
    horaLlegadaCtrl.dispose();
    inicioCargaCtrl.dispose();
    finCargaCtrl.dispose();
    horaDescargaCtrl.dispose();
    carguioKmCtrl.dispose();
    descargaKmCtrl.dispose();
    nivelCtrl.dispose();
    laborCtrl.dispose();
    destinoCtrl.dispose();
    materialCtrl.dispose();
    equipoCarguioObsCtrl.dispose();
    super.dispose();
  }

  Future<void> _seleccionarHora(TextEditingController controller) async {
    final now = TimeOfDay.now();
    final picked = await showTimePicker(
      context: context,
      initialTime: now,
    );

    if (picked != null) {
      final hour12 = picked.hourOfPeriod == 0 ? 12 : picked.hourOfPeriod;
      final minuteStr = picked.minute.toString().padLeft(2, '0');
      final hourStr = hour12.toString().padLeft(2, '0');
      final period = picked.period == DayPeriod.am ? 'AM' : 'PM';
      controller.text = '$hourStr:$minuteStr $period';
    }
  }

  void _agregarViaje() {
    if (nroViajeCtrl.text.trim().isEmpty) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Ingresa al menos el N° de viaje.')),
      );
      return;
    }

    if (codigoSeleccionado == null) {
      ScaffoldMessenger.of(context).showSnackBar(
        const SnackBar(content: Text('Selecciona un código de actividad.')),
      );
      return;
    }

    final viaje = Viaje(
      id: DateTime.now().millisecondsSinceEpoch.toString(),
      nroViaje: nroViajeCtrl.text.trim(),
      horaLlegada: horaLlegadaCtrl.text.trim(),
      inicioCarga: inicioCargaCtrl.text.trim(),
      finCarga: finCargaCtrl.text.trim(),
      horaDescarga: horaDescargaCtrl.text.trim(),
      carguioKm: carguioKmCtrl.text.trim(),
      descargaKm: descargaKmCtrl.text.trim(),
      codigoActividad: codigoSeleccionado ?? '',
      nivel: nivelCtrl.text.trim(),
      labor: laborCtrl.text.trim(),
      destino: destinoCtrl.text.trim(),
      material: materialCtrl.text.trim(),
      equipoCarguioObs: equipoCarguioObsCtrl.text.trim(),
    );

    setState(() {
      viajes.add(viaje);
    });

    _limpiarFormulario();
  }

  void _limpiarFormulario() {
    nroViajeCtrl.clear();
    horaLlegadaCtrl.clear();
    inicioCargaCtrl.clear();
    finCargaCtrl.clear();
    horaDescargaCtrl.clear();
    carguioKmCtrl.clear();
    descargaKmCtrl.clear();
    nivelCtrl.clear();
    laborCtrl.clear();
    destinoCtrl.clear();
    materialCtrl.clear();
    equipoCarguioObsCtrl.clear();
    codigoSeleccionado = null;
  }

  void _eliminarViaje(String id) {
    setState(() {
      viajes.removeWhere((v) => v.id == id);
    });
  }

  String _descripcionCodigo(String codigo) {
    final found = codigosActividad.firstWhere(
      (c) => c.codigo == codigo,
      orElse: () => CodigoActividad(codigo: '', descripcion: ''),
    );
    return found.descripcion;
  }

  String get _labelHoraLlegada {
    if (codigoSeleccionado == '11' ||
        codigoSeleccionado == '12' ||
        codigoSeleccionado == '13') {
      return 'Inicio';
    }
    if (codigoSeleccionado == '14') {
      return 'Hora de salida de parqueo';
    }
    return 'Hora de llegada';
  }

  String get _labelInicioCarguio {
    if (codigoSeleccionado == '11' ||
        codigoSeleccionado == '12' ||
        codigoSeleccionado == '13') {
      return 'Fin';
    }
    if (codigoSeleccionado == '14') {
      return 'Hora de llegada a la labor';
    }
    return 'Inicio de carguío';
  }

  Widget _campoTexto({
    required String label,
    required TextEditingController controller,
    String? hint,
    TextInputType keyboardType = TextInputType.text,
    void Function()? onTap,
    Widget? suffixIcon,
  }) {
    return Padding(
      padding: const EdgeInsets.only(bottom: 8),
      child: TextField(
        controller: controller,
        keyboardType: keyboardType,
        style: const TextStyle(fontSize: 13, color: Color(0xFFF9FAFB)),
        onTap: onTap,
        decoration: InputDecoration(
          labelText: label,
          hintText: hint,
          suffixIcon: suffixIcon,
        ),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    final dg = widget.datosGenerales;

    return Scaffold(
      appBar: AppBar(
        title: const Text('Registro de viajes'),
        centerTitle: true,
        backgroundColor: const Color(0xFF020617),
      ),
      body: SafeArea(
        child: SingleChildScrollView(
          padding: const EdgeInsets.all(16),
          child: Column(
            children: [
              Container(
                width: double.infinity,
                padding: const EdgeInsets.all(12),
                margin: const EdgeInsets.only(bottom: 16),
                decoration: BoxDecoration(
                  color: const Color(0xFF111827),
                  borderRadius: BorderRadius.circular(10),
                  border: Border.all(color: const Color(0xFF1F2937)),
                ),
                child: Column(
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    const Text(
                      'Datos generales',
                      style: TextStyle(
                        fontSize: 16,
                        fontWeight: FontWeight.w600,
                        color: Color(0xFFFACC15),
                      ),
                    ),
                    const SizedBox(height: 6),
                    Text('Fecha: ${dg.fecha}',
                        style: const TextStyle(color: Colors.white70)),
                    Text('Operador: ${dg.nombre}',
                        style: const TextStyle(color: Colors.white70)),
                    Text('Zona: ${dg.zona}  |  Turno: ${dg.turno}',
                        style: const TextStyle(color: Colors.white70)),
                    Text('Horómetro: ${dg.horometro}  |  Km: ${dg.kilometraje}',
                        style: const TextStyle(color: Colors.white70)),
                    Text('Combustible: ${dg.combustible}',
                        style: const TextStyle(color: Colors.white70)),
                    Text('Unidad: ${dg.codigoUnidad}',
                        style: const TextStyle(color: Colors.white70)),
                  ],
                ),
              ),
              const Align(
                alignment: Alignment.centerLeft,
                child: Text(
                  'Registrar viaje',
                  style: TextStyle(
                    fontSize: 18,
                    fontWeight: FontWeight.w600,
                    color: Color(0xFFFDE68A),
                  ),
                ),
              ),
              const SizedBox(height: 8),
              _campoTexto(
                label: 'N° viaje',
                controller: nroViajeCtrl,
                hint: '1',
                keyboardType: TextInputType.number,
              ),
              _campoTexto(
                label: _labelHoraLlegada,
                controller: horaLlegadaCtrl,
                hint: 'hh:mm AM/PM',
                keyboardType: TextInputType.datetime,
                onTap: () => _seleccionarHora(horaLlegadaCtrl),
                suffixIcon: const Icon(Icons.access_time),
              ),
              _campoTexto(
                label: _labelInicioCarguio,
                controller: inicioCargaCtrl,
                hint: 'hh:mm AM/PM',
                keyboardType: TextInputType.datetime,
                onTap: () => _seleccionarHora(inicioCargaCtrl),
                suffixIcon: const Icon(Icons.access_time),
              ),
              _campoTexto(
                label: 'Fin de carguío',
                controller: finCargaCtrl,
                hint: 'hh:mm AM/PM',
                keyboardType: TextInputType.datetime,
                onTap: () => _seleccionarHora(finCargaCtrl),
                suffixIcon: const Icon(Icons.access_time),
              ),
              _campoTexto(
                label: 'Hora de descarga',
                controller: horaDescargaCtrl,
                hint: 'hh:mm AM/PM',
                keyboardType: TextInputType.datetime,
                onTap: () => _seleccionarHora(horaDescargaCtrl),
                suffixIcon: const Icon(Icons.access_time),
              ),
              _campoTexto(
                label: 'Carguío (km)',
                controller: carguioKmCtrl,
                hint: 'km',
                keyboardType:
                    const TextInputType.numberWithOptions(decimal: true),
              ),
              _campoTexto(
                label: 'Descarga (km)',
                controller: descargaKmCtrl,
                hint: 'km',
                keyboardType:
                    const TextInputType.numberWithOptions(decimal: true),
              ),
              Padding(
                padding: const EdgeInsets.only(bottom: 8),
                child: DropdownButtonFormField<String>(
                  value: codigoSeleccionado,
                  dropdownColor: const Color(0xFF111827),
                  decoration: const InputDecoration(
                    labelText: 'Código actividad',
                  ),
                  items: codigosActividad
                      .map(
                        (c) => DropdownMenuItem(
                          value: c.codigo,
                          child: Text('${c.codigo} - ${c.descripcion}'),
                        ),
                      )
                      .toList(),
                  onChanged: (value) {
                    setState(() {
                      codigoSeleccionado = value;
                    });
                  },
                ),
              ),
              _campoTexto(
                label: 'Nivel',
                controller: nivelCtrl,
                hint: 'Ej. 850',
              ),
              _campoTexto(
                label: 'Labor',
                controller: laborCtrl,
                hint: 'Nombre de labor',
              ),
              _campoTexto(
                label: 'Destino',
                controller: destinoCtrl,
                hint: 'Planta / Ch. / etc.',
              ),
              _campoTexto(
                label: 'Material',
                controller: materialCtrl,
                hint: 'Mineral / Desmonte / etc.',
              ),
              _campoTexto(
                label: 'Eq. carguío / Observaciones',
                controller: equipoCarguioObsCtrl,
                hint: 'Scoop, cargador, observaciones...',
              ),
              const SizedBox(height: 10),
              SizedBox(
                width: double.infinity,
                child: ElevatedButton(
                  onPressed: _agregarViaje,
                  style: ElevatedButton.styleFrom(
                    backgroundColor: const Color(0xFF22C55E),
                    foregroundColor: const Color(0xFF052E16),
                    padding: const EdgeInsets.symmetric(vertical: 12),
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(10),
                    ),
                  ),
                  child: const Text(
                    'Agregar viaje',
                    style: TextStyle(
                      fontWeight: FontWeight.w700,
                      fontSize: 14,
                    ),
                  ),
                ),
              ),
              const SizedBox(height: 20),
              const Align(
                alignment: Alignment.centerLeft,
                child: Text(
                  'Viajes registrados',
                  style: TextStyle(
                    fontSize: 18,
                    fontWeight: FontWeight.w600,
                    color: Color(0xFFFDE68A),
                  ),
                ),
              ),
              const SizedBox(height: 8),
              if (viajes.isEmpty)
                const Align(
                  alignment: Alignment.centerLeft,
                  child: Text(
                    'Aún no hay viajes registrados.',
                    style: TextStyle(
                      color: Color(0xFF9CA3AF),
                      fontSize: 13,
                    ),
                  ),
                )
              else
                ListView.builder(
                  shrinkWrap: true,
                  physics: const NeverScrollableScrollPhysics(),
                  itemCount: viajes.length,
                  itemBuilder: (context, index) {
                    final v = viajes[index];
                    final desc = _descripcionCodigo(v.codigoActividad);
                    return Container(
                      width: double.infinity,
                      margin: const EdgeInsets.only(bottom: 10),
                      padding: const EdgeInsets.all(12),
                      decoration: BoxDecoration(
                        color: const Color(0xFF111827),
                        borderRadius: BorderRadius.circular(8),
                        border: Border.all(color: Colors.white24),
                      ),
                      child: Column(
                        crossAxisAlignment: CrossAxisAlignment.start,
                        children: [
                          Row(
                            mainAxisAlignment:
                                MainAxisAlignment.spaceBetween,
                            children: [
                              Text(
                                'Viaje N° ${v.nroViaje}',
                                style: const TextStyle(
                                  fontSize: 16,
                                  fontWeight: FontWeight.bold,
                                  color: Color(0xFFFACC15),
                                ),
                              ),
                              TextButton(
                                onPressed: () => _eliminarViaje(v.id),
                                child: const Text(
                                  'Eliminar',
                                  style: TextStyle(
                                    color: Color(0xFFF97373),
                                    fontSize: 12,
                                    fontWeight: FontWeight.w600,
                                  ),
                                ),
                              ),
                            ],
                          ),
                          Text('$_labelHoraLlegada: ${v.horaLlegada}',
                              style: const TextStyle(
                                  color: Color(0xFFE5E7EB), fontSize: 12)),
                          Text('$_labelInicioCarguio: ${v.inicioCarga}',
                              style: const TextStyle(
                                  color: Color(0xFFE5E7EB), fontSize: 12)),
                          Text('Fin de carguío: ${v.finCarga}',
                              style: const TextStyle(
                                  color: Color(0xFFE5E7EB), fontSize: 12)),
                          Text('Hora de descarga: ${v.horaDescarga}',
                              style: const TextStyle(
                                  color: Color(0xFFE5E7EB), fontSize: 12)),
                          Text(
                            'Carguío (km): ${v.carguioKm}',
                            style: const TextStyle(
                                color: Color(0xFFE5E7EB), fontSize: 12),
                          ),
                          Text(
                            'Descarga (km): ${v.descargaKm}',
                            style: const TextStyle(
                                color: Color(0xFFE5E7EB), fontSize: 12),
                          ),
                          Text(
                            'Código: ${v.codigoActividad}'
                            '${desc.isNotEmpty ? " - $desc" : ""}',
                            style: const TextStyle(
                                color: Color(0xFFE5E7EB), fontSize: 12),
                          ),
                          Text('Nivel: ${v.nivel}',
                              style: const TextStyle(
                                  color: Color(0xFFE5E7EB), fontSize: 12)),
                          Text('Labor: ${v.labor}',
                              style: const TextStyle(
                                  color: Color(0xFFE5E7EB), fontSize: 12)),
                          Text('Destino: ${v.destino}',
                              style: const TextStyle(
                                  color: Color(0xFFE5E7EB), fontSize: 12)),
                          Text('Material: ${v.material}',
                              style: const TextStyle(
                                  color: Color(0xFFE5E7EB), fontSize: 12)),
                          Text('Obs: ${v.equipoCarguioObs}',
                              style: const TextStyle(
                                  color: Color(0xFFE5E7EB), fontSize: 12)),
                        ],
                      ),
                    );
                  },
                ),
            ],
          ),
        ),
      ),
    );
  }
}
