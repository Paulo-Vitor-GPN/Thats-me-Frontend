const defaultRestaurants = ['Tatuapé', 'Móoca', 'Vila Prudente', 'Carrão', 'Ipiranga', 'Saúde', 'Água Rasa', 'Jabaquara', 'Butantã', 'Santo Amaro', 'Cursino', 'Limão', 'Vila Matilde', 'Jardim São Paulo', 'Pirituba', 'Vila Carrão', 'São Miguel Paulista', 'Sapopemba', 'Vila Guilherme', 'Vila Andrade', 'Vila Formosa', 'Tucuruvi', 'Freguesia do Ó', 'Campo Limpo', 'Cidade Ademar', 'Vila Jacuí', 'Vila Medeiros', 'Penha', 'Jardim Helena', 'Brás'];

const colors = generateColors(defaultRestaurants.length);

function generateColors(quantity) {
  const generatedColors = [];

  for (let i = 0; i < quantity; i++) {
    // Alternar entre as duas cores fornecidas
    const color = i % 2 === 0 ? '#86131C' : '#D9AE62';

    generatedColors.push(color);
  }

  return generatedColors;
}

console.log(colors);
