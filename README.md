Gym gym = new Gym();
bool open = true;
while (open)
{
    Console.WriteLine("Welcome to the club body");
    Console.WriteLine("Select an option:");
    Console.WriteLine("1. Add a new Coach");
    Console.WriteLine("2. Add a new Client");
    Console.WriteLine("3. Assign client to coach");
    Console.WriteLine("4. Show all the coaches and clients");
    Console.WriteLine("5. Find the coach by client name");
    var input = Convert.ToInt32(Console.ReadLine());
    switch (input)
    {
        case 1:
        {
            Console.WriteLine("Enter the name of the coach");
            var name = Console.ReadLine();
            gym.AddCoach(new Coach(name));
            break;
        }
        case 2:
        {
            Console.WriteLine("Enter the name of the client");
            var name = Console.ReadLine();
            gym.AddClient(new Client(name));
            break;
        }

        case 3:
        {
            Console.WriteLine("Enter the name of the client");
            var name = Console.ReadLine();
            var client = gym.Clients.FirstOrDefault(x => x.Name == name);
            if (client == null)
            {
                Console.WriteLine("Client not found");
                break;
            }

            Console.WriteLine("Enter the name of the coach");
            var coachName = Console.ReadLine();
            var coach = gym.Coaches.FirstOrDefault(x => x.Name == coachName);
            if (coach == null)
            {
                Console.WriteLine("Coach not found");
                break;
            }

            gym.AssignClientToCoach(coach, client);
            break;
        }
        case 4:
        {
            foreach (var person in gym.Coaches)
            {
                Console.WriteLine(person.Name);
            }

            Console.WriteLine("Enter the name of the coach");
            string answer = Console.ReadLine();
            var coach = gym.Coaches.FirstOrDefault(x => x.Name == answer);
            if (coach == null)
            {
                Console.WriteLine("Coach not found");
                break;
            }

            foreach (var person1 in coach.Clients)
            {
                Console.WriteLine(person1.Name);
            }

            break;
        }
        case 5:
        {
            Console.WriteLine("Enter the name of the client");
            string answer = Console.ReadLine();
            var client = gym.Clients.FirstOrDefault(x => x.Name == answer);
            if (client == null)
            {
                Console.WriteLine("Client not found");
                break;
            }

            if (client.Coach == null)
            {
                Console.WriteLine("This client don't have a coach");
            }
            

            Console.WriteLine(client.Coach.Name);
            break;
        }
    }
}



public class Gym
{
    private readonly List<Client> _clients = [];
    public  IReadOnlyCollection<Client> Clients =>_clients;
    private readonly List<Coach> _coaches = [];
    public IReadOnlyCollection<Coach> Coaches => _coaches;

    public void AddClient(Client client)
    {
        _clients.Add(client);
    }

    public void AddCoach(Coach coach)
    {
        _coaches.Add(coach);
    }

    public void AssignClientToCoach(Coach coach, Client client)
    {
        coach.AddClient(client);
        client.Coach = coach;
    }

    
    
}

public class Coach(string name)
{
    private readonly List<Client> _clients = [];
    
    public string Name { get; } = name;
    public  IReadOnlyCollection<Client> Clients =>_clients;
    
    public void AddClient(Client client)
    {
        ArgumentNullException.ThrowIfNull(client);
        _clients.Add(client);
    }
}

public class Client
{
    public string Name { get;  }
    public Coach Coach { get; set; }

    public Client(string name)
    {
        Name = name;
    }
}
