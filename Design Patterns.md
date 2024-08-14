
### Abstract Factory

lets you produce families of related objects without specifying their concrete classes.

```typescript
interface Movie {
  title: string;
}

interface Audio {
  artist : string;
}

interface MovieApi {
  searchByTitle: (title: string) => Movie[];
}

interface AudioApi {
  searchByArtist: (artist: string) => AudioTrack[];
}

class NormalMovieApiProvider implements MovieApi {
  // Implementation
}

class NormalAudioApiProvider implements AudioApi {
  // Implementation
}

class PremiumMovieApiProvider implements MovieApi {
  // Implementation
}

class PremiumAudioApiProvider implements AudioApi {
  // Implementation
}

interface ApiProviderFactory {
  createMovieApiProvider: () => MovieApi;
  createAudioApiProvider: () => AudioApi;
}

class NormalApiProviderFactory implements ApiProviderFactory {
  createMovieApiProvider() {
    return new NormalMovieApiProvider();
  }

  createAudioApiProvider() {
    return new NormalAudioApiProvider();
  }
}

class PremiumApiProviderFactory implements ApiProviderFactory {
  createMovieApiProvider() {
    return new PremiumMovieApiProvider();
  }

  createAudioApiProvider() {
    return new PremiumAudioApiProvider();
  }
}
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Builder

> Builder is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.

<img src="https://user-images.githubusercontent.com/37804060/165857814-d11b7310-ec21-4596-9d53-26fc2acf1a57.png"/>

```typescript
class Page {
  private headerParts: string[];
  private bodyParts: string[];
  private footerParts: string[];

  constructor() {
    this.headerParts = [];
    this.bodyParts = [];
    this.footerParts = [];
  }

  public setHeaderParts(...parts: string[]) {
    this.headerParts = parts;
  }

  public setBodyParts(...parts: string[]) {
    this.bodyParts = parts;
  }

  public setFooterParts(...parts: string[]) {
    this.footerParts = parts;
  }

  public getPage() {
    return {
      headerParts: this.headerParts,
      bodyParts: this.bodyParts,
      footerParts: this.footerParts,
    };
  }
}

interface PageBuilder {
  getPage: () => Page;
  buildHeader: () => void;
  buildBody: () => void;
  buildFooter: () => void;
}

class PersonalBlogPageBuilder implements PageBuilder {
  private page: Page;

  constructor() {
    this.page = new Page();
  }

  public getPage() {
    return this.page;
  }

  public buildHeader() {
    this.page.setHeaderParts("Title", "Author Information");
  }

  public buildBody() {
    this.page.setBodyParts("Recent Posts", "Favorite Posts", "Last Comments");
  }

  public buildFooter() {
    this.page.setFooterParts("CopyRights", "Author Email Address");
  }
}

class OnlineShopPageBuilder implements PageBuilder {
  private page: Page;

  constructor() {
    this.page = new Page();
  }

  public getPage() {
    return this.page;
  }

  public buildHeader() {
    this.page.setHeaderParts("Logo", "Description", "Products Category Menu");
  }

  public buildBody() {
    this.page.setBodyParts("New Products", "Daily Off", "Suggested Products");
  }

  public buildFooter() {
    this.page.setFooterParts("About Us", "Address", "Legal Certificate Link");
  }
}
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Factory Method

> Factory Method is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

<img src="https://user-images.githubusercontent.com/37804060/165857922-b719ba3a-458c-4761-8e98-f0d05a93940c.png"/>

```typescript
enum PaymentType {
  Paypal = "PAYPAL",
  Bitcoin = "BITCOIN",
  VisaCard = "VISA_CARD",
}

abstract class PaymentService {
  public abstract payMoney(amount: number): void;
}

class Paypal extends PaymentService {
  public override payMoney(amount: number) {
    console.log(`You paid ${amount} dollars by Paypal.`);
  }
}

class Bitcoin extends PaymentService {
  public override payMoney(amount: number) {
    console.log(`You paid ${amount} dollars by Bitcoin.`);
  }
}

class VisaCard extends PaymentService {
  public override payMoney(amount: number) {
    console.log(`You paid ${amount} dollars by VisaCard.`);
  }
}

abstract class PaymentFactory {
  public abstract createService(): PaymentService;
}

class PaypalFactory extends PaymentFactory {
  public override createService(): PaymentService {
    return new Paypal();
  }
}

class BitcoinFactory extends PaymentFactory {
  public override createService(): PaymentService {
    return new Bitcoin();
  }
}

class VisaCardFactory extends PaymentFactory {
  public override createService(): PaymentService {
    return new VisaCard();
  }
}

// Usage

function getPaymentFactory(paymentType: PaymentType): PaymentFactory {
  switch (paymentType) {
    case PaymentType.Paypal:
      return new PaypalFactory();
    case PaymentType.Bitcoin:
      return new BitcoinFactory();
    case PaymentType.VisaCard:
      return new VisaCardFactory();
    default:
      throw new Error("Invalid payment type.");
  }
}

const paypalService = getPaymentFactory(PaymentType.Paypal).createService();
paypalService.payMoney(100); // You paid 100 dollars by Paypal.

const bitcoinService = getPaymentFactory(PaymentType.Bitcoin).createService();
bitcoinService.payMoney(200); // You paid 200 dollars by Bitcoin.

const visaCardService = getPaymentFactory(PaymentType.VisaCard).createService();
visaCardService.payMoney(300); // You paid 300 dollars by VisaCard.
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Prototype

> Prototype is a creational design pattern that lets you copy existing objects without making your code dependent on their classes.

<img src="https://user-images.githubusercontent.com/37804060/165857765-643633b0-72eb-43ed-9f0c-9dca50c0939b.png"/>

```typescript
interface IPrototype {
  clone: () => IPrototype;
}

class Product implements IPrototype {
  private name: string;
  private price: number;
  private warranty: Date | null;

  constructor(name: string, price: number, warranty: Date | null) {
    this.name = name;
    this.price = price;
    this.warranty = warranty;
  }

  // Assume we implement methods, getters and setters all here

  public clone() {
    return new Product(this.name, this.price, this.warranty);
  }
}

const productOne = new Product("Laptop", 2500000, new Date(2050));

const productTwo = productOne.clone();

// productOne !== productTwo but their properties are the same
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Singleton

> Singleton is a creational design pattern that lets you ensure that a class has only one instance, while providing a global access point to this instance.

<img src="https://user-images.githubusercontent.com/37804060/165857698-b1cbb582-4e8c-4cb7-af89-b11edc687626.png"/>

```typescript
class Weather {
  private static instance: Weather | null = null;

  private statusOfCities: {
    city: string;
    status: "SUNNY" | "CLOUDY" | "RAINY" | "SNOWY";
  }[];

  private constructor() {
    const data = []; // Get data from API
    this.statusOfCities = data;
  }

  public getTemperatureByCity(city: string) {
    return this.statusOfCities.find((data) => data.city === city);
  }

  public static getInstance() {
    if (this.instance == null) {
      this.instance = new Weather();
    }

    return this.instance;
  }
}

const instanceOne = Weather.getInstance();
const instanceTwo = Weather.getInstance();
// instanceOne is equal to instanceTwo (instanceOne === instanceTwo)
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Adapter (Wrapper)

> Adapter is a structural design pattern that allows objects with incompatible interfaces to collaborate.

<img src="https://user-images.githubusercontent.com/37804060/165858390-a0bdcf53-22bb-42b3-b499-70b51384a61c.png"/>

```typescript
interface StandardUser {
  fullName: string;
  skills: string[];
  age: number;
  contact: {
    email: string;
    phone: string;
  };
}

abstract class ResumeServiceApi {
  static generateResume(data: StandardUser) {
    /* Implementation */
  }
}

class User {
  readonly firstName: string;
  readonly lastName: string;
  readonly birthday: Date;
  readonly skills: Record<string, 1 | 2 | 3 | 4 | 5>;
  readonly email?: string;
  readonly phone?: string;

  constructor({
    firstName,
    lastName,
    birthday,
    skills,
    email,
    phone,
  }: {
    firstName: string;
    lastName: string;
    birthday: Date;
    skills: Record<string, 1 | 2 | 3 | 4 | 5>;
    email?: string;
    phone?: string;
  }) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.birthday = birthday;
    this.skills = skills;
    this.email = email;
    this.phone = phone;
  }
}

class UserAdapter implements StandardUser {
  private user: User;

  constructor(user: User) {
    this.user = user;
  }

  get fullName() {
    return `${this.user.firstName} ${this.user.lastName}`;
  }

  get skills() {
    return Object.keys(this.user.skills);
  }

  get age() {
    return new Date().getFullYear() - this.user.birthday.getFullYear();
  }

  get contact() {
    return { email: this.user.email ?? "", phone: this.user.phone ?? "" };
  }
}

// Usage

const user = new User({
  firstName: "Ahmad",
  lastName: "Jafari",
  birthday: new Date(1999, 1, 1, 0, 0, 0, 0),
  skills: { TypeScript: 4, JavaScript: 3, OOP: 4, CSharp: 2, Java: 1 },
  email: "a99jafari@gmail.com",
  phone: "+98 930 848 XXXX",
});

const resume = ResumeServiceApi.generateResume(user); // ==> Type Error!

const standardUser = new UserAdapter(user);
const resume = ResumeServiceApi.generateResume(standardUser); // ==> OK!
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Bridge

> Bridge is a structural design pattern that lets you split a large class or a set of closely related classes into two separate hierarchies—abstraction and implementation—which can be developed independently of each other.

<img src="https://user-images.githubusercontent.com/37804060/165858360-9b8ab73c-9b08-41d1-b7b9-84a2e5ef5028.png"/>

```typescript
interface Player {
  play(): string;
  stop(): string;
}

class AudioPlayer implements Player {
  play(): string {
    return "Audio is playing";
  }

  stop(): string {
    return "Audio is stopped";
  }
}

class VideoPlayer implements Player {
  play(): string {
    return "Video is playing";
  }

  stop(): string {
    return "Video is stopped";
  }
}

interface Platform {
  play(): string;
  stop(): string;
}

class Desktop implements Platform {
  private player: Player;

  constructor(player: Player) {
    this.player = player;
  }

  play(): string {
    return this.player.play() + " on desktop";
  }

  stop(): string {
    return this.player.stop() + " on desktop";
  }
}

class Mobile implements Platform {
  private player: Player;

  constructor(player: Player) {
    this.player = player;
  }

  play(): string {
    return this.player.play() + " on mobile";
  }

  stop(): string {
    return this.player.stop() + " on mobile";
  }
}

// Usage
const audioPlayer = new AudioPlayer();
const videoPlayer = new VideoPlayer();

const desktopVideoPlayer = new Desktop(videoPlayer);
const desktopAudioPlayer = new Desktop(audioPlayer);
const mobileVideoPlayer = new Mobile(videoPlayer);
const mobileAudioPlayer = new Mobile(audioPlayer);
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Composite (Object Tree)

> Composite is a structural design pattern that lets you compose objects into tree structures and then work with these structures as if they were individual objects.

<img src="https://user-images.githubusercontent.com/37804060/165858367-a2ba0672-4bb3-4b11-a63c-7843df0cfb7c.png"/>

```typescript
abstract class BaseUnit<T> {
  constructor(
    private readonly id: string,
    private readonly units: BaseUnit<T>[] = []
  ) {}

  getUnit(unitId: string): BaseUnit<T> | null {
    return this.units.find((unit) => unit.id === unitId) ?? null;
  }

  getSalary(): number {
    return this.units.reduce((acc, unit) => acc + unit.getSalary(), 0);
  }

  increaseSalary(percentage: number): void {
    this.units.forEach((unit) => unit.increaseSalary(percentage));
  }
}

class Employee extends BaseUnit<null> {
  private salary: number;

  constructor(id: string, salary: number) {
    super(id);
    this.salary = salary;
  }

  getUnit(): never {
    throw new Error("Employee cannot have sub-units");
  }

  getSalary() {
    return this.salary;
  }

  increaseSalary(percentage: number) {
    this.salary = this.salary + (this.salary * percentage) / 100;
  }
}

class Department extends BaseUnit<Employee> {}

class Faculty extends BaseUnit<Department> {}

class University extends BaseUnit<Faculty> {}

// Usage

const harvardUniversity = new University("Harvard", [
  new Faculty("Engineering", [
    new Department("Computer", [
      new Employee("C1", 6200),
      new Employee("C2", 5400),
      new Employee("C3", 5600),
    ]),
    new Department("Electrical", [
      new Employee("E1", 4800),
      new Employee("E2", 5800),
    ]),
  ]),
  new Faculty("Science", [
    new Department("Physics", [
      new Employee("P1", 3800),
      new Employee("P2", 4600),
    ]),
    new Department("Mathematics", [
      new Employee("M1", 5200),
      new Employee("M2", 5600),
      new Employee("M3", 4600),
    ]),
  ]),
]);

console.log(harvardUniversity.getSalary());
harvardUniversity.increaseSalary(10);
console.log(harvardUniversity.getSalary());

const engineeringFaculty = harvardUniversity.getUnit("Engineering") as Faculty;
console.log(engineeringFaculty.getSalary());
engineeringFaculty.increaseSalary(10);
console.log(engineeringFaculty.getSalary());

const computerDepartment = engineeringFaculty.getUnit("Computer") as Department;
console.log(computerDepartment.getSalary());
computerDepartment.increaseSalary(10);
console.log(computerDepartment.getSalary());

const employee = computerDepartment.getUnit("C1") as Employee;
console.log(employee.getSalary());
employee.increaseSalary(10);
console.log(employee.getSalary());
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Decorator (Wrapper)

> Adapter is a structural design pattern that allows objects with incompatible interfaces to collaborate.

<img src="https://user-images.githubusercontent.com/37804060/165858382-6ece64aa-4c9f-4e4e-944e-f7a67fcdd162.png"/>

```typescript
interface INotifier {
  sendMessage: (message: string) => void;
  setUsers: (users: string[]) => void;
  getUsers: () => string[];
}

class Notifier implements INotifier {
  private users: string[];

  constructor(users: string[]) {
    this.users = users;
  }

  public sendMessage(message: string) {
    this.users.forEach((user) => {
      // Show the `message` to the `user` on Web Application
    });
  }

  public setUsers(users: string[]) {
    this.users = users;
  }

  public getUsers() {
    return this.users;
  }
}

abstract class NotifierDecorator implements INotifier {
  protected notifier: INotifier;

  constructor(notifier: INotifier) {
    this.notifier = notifier;
  }

  public abstract sendMessage(message: string);

  public getUsers() {
    return this.notifier.getUsers();
  }

  public setUsers(users: string[]) {
    this.notifier.setUsers(users);
  }
}

class EmailNotifier extends NotifierDecorator {
  sendMessage(message: string) {
    notifier.getUsers().forEach((user) => {
      // Send the `message` to the `user` via Email
    });

    this.notifier.sendMessage(message);
  }
}

class SlackNotifier extends NotifierDecorator {
  sendMessage(message: string) {
    this.notifier.getUsers().forEach((user) => {
      // Send the `message` to the `user` via Slack
    });

    this.notifier.sendMessage(message);
  }
}

class SmsNotifier extends NotifierDecorator {
  sendMessage(message: string) {
    this.notifier.getUsers().forEach((user) => {
      // Send the `message` to the `user` via SMS
    });

    this.notifier.sendMessage(message);
  }
}

const notifier = new Notifier(["Ahmad", "Artin", "Ghazaleh"]);

const notifierByEmail = new EmailNotifier(notifier);
const notifierBySlack = new SlackNotifier(notifier);
const notifierBySMS = new SmsNotifier(notifier);

const notifierByEmailAndSlack = new EmailNotifier(new SlackNotifier(notifier));
const notifierByEmailAndSMS = new EmailNotifier(new SmsNotifier(notifier));
const notifierBySlackAndSMS = new SlackNotifier(new SmsNotifier(notifier));

const notifierByEmailAndSlackAndSMS = new EmailNotifier(
  new SlackNotifier(new SmsNotifier(notifier))
);
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Facade

> Facade is a structural design pattern that provides a simplified interface to a library, a framework, or any other complex set of classes.

<img src="https://user-images.githubusercontent.com/37804060/165858377-16fc76a5-3b79-4837-bf3c-3f38539a4ac3.png"/>

```typescript
class GitChecker {
  private repositoryPath: string;

  constructor(repositoryPath: string) {
    this.repositoryPath = repositoryPath;
  }

  analyzeCommits() {
    // Checks the quality of commit messages
  }

  analyzeUnmergedBranches() {
    // Checks the
  }
}

class Linter {
  private rules: string[];

  constructor(rules: string[]) {
    this.rules = rules;
  }

  findIssues() {
    // Checks codebase and finds all issues
  }

  resolveFixableIssues() {
    // Checks codebase and fix all fixable issues
  }
}

class PackageManager {
  private dependencies: { name: string; version: number }[];

  constructor(dependencies: { name: string; version: number }[]) {
    this.dependencies = dependencies;
  }

  findUnsecureLibraries() {
    // Analyzes all dependencies and finds all of unsecure libraries
  }

  findDeprecatedLibraries() {
    // Analyzes all dependencies and finds all of deprecated libraries
  }
}

// Facade Class
class CodebaseAnalyzer {
  private gitChecker: GitChecker;
  private linter: Linter;
  private packageManager: PackageManager;

  constructor({
    repositoryPath,
    linterRules,
    dependencies,
  }: {
    repositoryPath: string;
    linterRules: string[];
    dependencies: { name: string; version: number }[];
  }) {
    this.gitChecker = new GitChecker(repositoryPath);
    this.linter = new Linter(linterRules);
    this.packageManager = new PackageManager(dependencies);
  }

  // This method is the facade method and does all of the work
  analyze() {
    this.gitChecker.analyzeCommits();
    this.gitChecker.analyzeUnmergedBranches();
    this.linter.findIssues();
    this.linter.resolveFixableIssues();
    this.packageManager.findUnsecureLibraries();
    this.packageManager.findDeprecatedLibraries();
  }
}

// Usage
const codebaseAnalyzer = new CodebaseAnalyzer({
  repositoryPath: "root/design-patterns/structural/facade/",
  linterRules: ["rule1", "rule2", "rule3", "rule4"],
  dependencies: [
    { name: "ABC", version: 19 },
    { name: "MNP", version: 14 },
    { name: "XYZ", version: 23 },
  ],
});

codebaseAnalyzer.analyze();
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Flyweight (Cache)

> Flyweight is a structural design pattern that lets you fit more objects into the available amount of RAM by sharing common parts of state between multiple objects instead of keeping all of the data in each object.

<img src="https://user-images.githubusercontent.com/37804060/165858370-1ec33962-9bad-4f5d-8078-132bf7d9da29.png"/>

```typescript
interface IRequest {
  readonly method: "GET" | "POST" | "PUT" | "DELETE";
  readonly url: string;
  readonly body: Record<string, string>;
  send(): Promise<any>;
}

class MinimalRequest implements IRequest {
  constructor(
    public readonly method: "GET" | "POST" | "PUT" | "DELETE",
    public readonly url: string,
    public readonly body: Record<string, string> = {}
  ) {}

  public async send(): Promise<any> {
    const options = { method: this.method, body: JSON.stringify(this.body) };

    const response = await fetch(this.url, options);

    return response.json();
  }
}

class RequestFactory {
  private requests: Map<string, IRequest> = new Map();

  public createRequest(
    method: "GET" | "POST" | "PUT" | "DELETE",
    url: string,
    body: Record<string, string> = {}
  ): IRequest {
    const key = `${method}-${url}`;

    if (!this.requests.has(key)) {
      const request = new MinimalRequest(method, url, body);
      this.requests.set(key, request);
    }

    return this.requests.get(key)!; // Type assertion for clarity
  }
}

class ParallelRequestsHandler {
  private factory: RequestFactory;

  constructor(factory: RequestFactory) {
    this.factory = factory;
  }

  public async sendAll(
    requestsInfo: {
      method: "GET" | "POST" | "PUT" | "DELETE";
      url: string;
      body?: Record<string, string>;
    }[]
  ): Promise<any[]> {
    const requests = requestsInfo.map((requestInfo) =>
      this.factory.createRequest(
        requestInfo.method,
        requestInfo.url,
        requestInfo.body
      )
    );
    const responses = await Promise.all(
      requests.map((request) => request.send())
    );

    return responses;
  }
}
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Proxy

> Proxy is a structural design pattern that lets you provide a substitute or placeholder for another object. A proxy controls access to the original object, allowing you to perform something either before or after the request gets through to the original object.

<img src="https://user-images.githubusercontent.com/37804060/165858391-e373f505-107c-492d-b354-6a10d6441e35.png"/>

```typescript
interface IRequestHandler {
  sendRequest(method: string, url: string, body?: string): void;
}

class RequestHandler implements IRequestHandler {
  sendRequest(method: string, url: string, body?: string): void {
    console.log(`Request sent: ${method} ${url} ${body}`);
  }
}

class RequestHandlerProxy implements IRequestHandler {
  private realApi: RequestHandler;

  constructor(realApi: RequestHandler) {
    this.realApi = realApi;
  }

  private logRequest(method: string, url: string, body?: string): void {
    console.log(`Request logged: ${method} ${url} ${body}`);
  }

  private validateRequestUrl(url: string): boolean {
    return url.startsWith("/api");
  }

  sendRequest(method: string, url: string, body?: string): void {
    if (this.validateRequestUrl(url)) {
      this.realApi.sendRequest(method, url, body);
      this.logRequest(method, url, body);
    }
  }
}

// Usage

const realRequestHandler = new RequestHandler();
const proxyRequestHandler = new RequestHandlerProxy(realRequestHandler);

proxyRequestHandler.sendRequest("GET", "/api/users");
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Chain of Responsibility

> Chain of Responsibility is a behavioral design pattern that lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain.

<img src="https://github.com/jafari-dev/oop-expert-with-typescript/assets/37804060/2bea3788-2ba7-49e9-87d2-a9aa8d81e43e"/>

```typescript
interface IResponse {
  statusCode: number;
  body: Record<string, unknown>;
  authentication: Record<string, string>;
  message?: string;
}

class ResponseHandler {
  private nextHandler?: ResponseHandler;

  protected process(response: IResponse) {
    return response;
  }

  public setNext(ResponseHandler: ResponseHandler): ResponseHandler {
    this.nextHandler = ResponseHandler;

    return ResponseHandler;
  }

  public handle(response: IResponse): IResponse {
    const processedResponse = this.process(response);

    if (this.nextHandler != null) {
      return this.nextHandler.handle(processedResponse);
    } else {
      return processedResponse;
    }
  }
}

class Encryptor extends ResponseHandler {
  private encryptTokens(response: IResponse) {
    const { authentication } = response;
    const encryptedAuthTokens: Record<string, string> = {};

    for (const key in authentication) {
      encryptedAuthTokens[key] = `encrypted-${authentication[key]}`;
    }

    return { ...response, authentication: encryptedAuthTokens };
  }

  protected process(response: IResponse) {
    const encryptedResponse = this.encryptTokens(response);

    return encryptedResponse;
  }
}

class BodyFormatter extends ResponseHandler {
  private transformKeysToCamelCase(body: Record<string, unknown>) {
    const newBody: Record<string, unknown> = {};

    for (const key in body) {
      const camelCaseKey = key.replace(/_([a-z])/g, (subString) =>
        subString[1].toUpperCase()
      );
      newBody[camelCaseKey] = body[key];
    }

    return newBody;
  }

  protected process(response: IResponse) {
    const clonedResponseBody = JSON.parse(JSON.stringify(response.body));
    const formattedBody = this.transformKeysToCamelCase(clonedResponseBody);
    const formattedResponse = { ...response, body: formattedBody };

    return formattedResponse;
  }
}

class MetadataAdder extends ResponseHandler {
  private getResponseMetadata(statusCode: number) {
    if (statusCode < 200) return "Informational";
    else if (statusCode < 300) return "Success";
    else if (statusCode < 400) return "Redirection";
    else if (statusCode < 500) return "Client Error";
    else return "Server Error";
  }

  protected process(response: IResponse) {
    const updatedResponse = {
      ...response,
      message: this.getResponseMetadata(response.statusCode),
    };

    return updatedResponse;
  }
}

// Usage
const response: IResponse = {
  statusCode: 200,
  body: {
    design_pattern_name: "Chain of Responsibility",
    pattern_category: "Behavioral",
    complexity_percentage: 80,
  },
  authentication: {
    api_token: "12345678",
    refresh_token: "ABCDEFGH",
  },
};

const responseHandler = new ResponseHandler();
const encryptor = new Encryptor();
const bodyFormatter = new BodyFormatter();
const metadataAdder = new MetadataAdder();

responseHandler
  .setNext(encryptor)
  .setNext(bodyFormatter)
  .setNext(metadataAdder);

const resultResponse = responseHandler.handle(response);

console.log(resultResponse);
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Command

> Command is a behavioral design pattern that turns a request into a stand-alone object that contains all information about the request. This transformation lets you pass requests as a method arguments, delay or queue a request’s execution, and support undoable operations.

<img src="https://github.com/jafari-dev/oop-expert-with-typescript/assets/37804060/c685b9d8-f7a5-4915-b75c-4e897fe70014"/>

```typescript
interface Command {
  execute(): void;
  undo(): void;
}

class AddTextCommand implements Command {
  private prevText: string;

  constructor(private editor: TextEditor, private text: string) {}

  execute() {
    this.prevText = this.editor.content;
    this.editor.content += this.text;
  }

  undo() {
    this.editor.content = this.prevText;
  }
}

class DeleteTextCommand implements Command {
  private prevText: string;

  constructor(private editor: TextEditor) {}

  execute() {
    this.prevText = this.editor.content;
    this.editor.content = "";
  }

  undo() {
    this.editor.content = this.prevText;
  }
}

class TextEditor {
  content: string = "";
}

class CommandInvoker {
  private commandHistory: Command[] = [];
  private currentCommandIndex: number = -1;

  executeCommand(command: Command) {
    if (this.currentCommandIndex < this.commandHistory.length - 1) {
      this.commandHistory = this.commandHistory.slice(
        0,
        this.currentCommandIndex + 1
      );
    }

    command.execute();
    this.commandHistory.push(command);
    this.currentCommandIndex++;
  }

  undo() {
    if (this.currentCommandIndex >= 0) {
      const command = this.commandHistory[this.currentCommandIndex];
      command.undo();
      this.currentCommandIndex--;
    } else {
      console.log("Nothing to undo.");
    }
  }

  redo() {
    if (this.currentCommandIndex < this.commandHistory.length - 1) {
      const command = this.commandHistory[this.currentCommandIndex + 1];
      command.execute();
      this.currentCommandIndex++;
    } else {
      console.log("Nothing to redo.");
    }
  }
}

// Client Code
const editor = new TextEditor();
const invoker = new CommandInvoker();

const addTextCmd = new AddTextCommand(editor, "Hello, World!");
invoker.executeCommand(addTextCmd);
console.log(editor.content); // "Hello, World!"

const deleteTextCmd = new DeleteTextCommand(editor);
invoker.executeCommand(deleteTextCmd);
console.log(editor.content); // ""

invoker.undo();
console.log(editor.content); // "Hello, World!"

invoker.redo();
console.log(editor.content); // ""
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Interpreter

> Interpreter is a behavioral design pattern that provides a way to interpret and evaluate sentences or expressions in a language. This pattern defines a language grammar, along with an interpreter that can parse and execute the expressions.

<img src="https://github.com/jafari-dev/oop-expert-with-typescript/assets/37804060/b5b36a91-dd3d-4bf5-a476-bae961c1d421"/>

```typescript
interface Expression {
  interpret(): number;
}

class NumberExpression implements Expression {
  constructor(private value: number) {}

  interpret(): number {
    return this.value;
  }
}

class PlusExpression implements Expression {
  constructor(private left: Expression, private right: Expression) {}

  interpret(): number {
    return this.left.interpret() + this.right.interpret();
  }
}

class MinusExpression implements Expression {
  constructor(private left: Expression, private right: Expression) {}

  interpret(): number {
    return this.left.interpret() - this.right.interpret();
  }
}

class MultiplyExpression implements Expression {
  constructor(private left: Expression, private right: Expression) {}

  interpret(): number {
    return this.left.interpret() * this.right.interpret();
  }
}

class DivideExpression implements Expression {
  constructor(private left: Expression, private right: Expression) {}

  interpret(): number {
    return this.left.interpret() / this.right.interpret();
  }
}

class Interpreter {
  interpret(expression: string): number {
    const stack: Expression[] = [];

    const tokens = expression.split(" ");
    for (const token of tokens) {
      if (this.isOperator(token)) {
        const right = stack.pop()!;
        const left = stack.pop()!;
        const operator = this.createExpression(token, left, right);
        stack.push(operator);
      } else {
        stack.push(new NumberExpression(parseFloat(token)));
      }
    }

    return stack.pop()!.interpret();
  }

  private isOperator(token: string): boolean {
    return token === "+" || token === "-" || token === "*" || token === "/";
  }

  private createExpression(
    operator: string,
    left: Expression,
    right: Expression
  ): Expression {
    switch (operator) {
      case "+":
        return new PlusExpression(left, right);
      case "-":
        return new MinusExpression(left, right);
      case "*":
        return new MultiplyExpression(left, right);
      case "/":
        return new DivideExpression(left, right);
      default:
        throw new Error("Invalid operator: " + operator);
    }
  }
}

// Usage
const interpreter = new Interpreter();

console.log(interpreter.interpret("3 4 +")); // Output: 7
console.log(interpreter.interpret("5 2 * 3 +")); // Output: 13
console.log(interpreter.interpret("10 2 /")); // Output: 5
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Iterator

> Iterator is a behavioral design pattern that lets you traverse elements of a collection without exposing its underlying representation (list, stack, tree, etc.).

<img src="https://github.com/jafari-dev/oop-expert-with-typescript/assets/37804060/20229eb8-caa0-4953-adf0-87f516332966"/>

```typescript
interface MyIterator<T> {
  hasPrevious: () => boolean;
  hasNext: () => boolean;
  previous: () => T;
  next: () => T;
}

class Book {
  readonly title: string;
  readonly author: string;
  readonly isbn: string;

  constructor(title: string, author: string, isbn: string = "") {
    this.title = title;
    this.author = author;
    this.isbn = isbn;
  }
}

class BookShelf {
  private books: Book[] = [];

  getLength(): number {
    return this.books.length;
  }

  addBook(book: Book): void {
    this.books.push(book);
  }

  getBookAt(index: number): Book {
    return this.books[index];
  }

  createIterator() {
    return new BookShelfIterator(this);
  }
}

class BookShelfIterator implements MyIterator<Book> {
  private bookShelf: BookShelf;
  private currentIndex: number;

  constructor(bookShelf: BookShelf) {
    this.bookShelf = bookShelf;
    this.currentIndex = 0;
  }

  hasNext() {
    return this.currentIndex < this.bookShelf.getLength();
  }

  hasPrevious() {
    return this.currentIndex > 0;
  }

  next() {
    this.currentIndex += 1;
    return this.bookShelf.getBookAt(this.currentIndex);
  }

  previous() {
    this.currentIndex -= 1;
    return this.bookShelf.getBookAt(this.currentIndex);
  }
}

// Usage
const shelf = new BookShelf();
shelf.addBook(new Book("Design Patterns", "Gang of Four"));
shelf.addBook(new Book("Clean Code", "Robert C. Martin"));
shelf.addBook(new Book("You Don't Know JS", "Kyle Simpson"));

const MyIterator = shelf.createIterator();

while (MyIterator.hasNext()) {
  const book = MyIterator.next();
  console.log(`${book.title} by ${book.author}`);
}
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Mediator

> Mediator is a behavioral design pattern that lets you reduce chaotic dependencies between objects. The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object.

<img src="https://github.com/jafari-dev/oop-expert-with-typescript/assets/37804060/e898a7aa-a40e-4679-af89-e41d26d77cfb"/>

```typescript
interface ChatMediator {
  sendMessage(receiver: User, message: string): void;
}

class ConcreteChatMediator implements ChatMediator {
  private users: User[] = [];

  addUser(user: User): void {
    this.users.push(user);
  }

  sendMessage(receiver: User, message: string): void {
    for (const user of this.users) {
      // Don't send the message to the user who sent it
      if (user !== receiver) {
        user.receiveMessage(message);
      }
    }
  }
}

class User {
  private mediator: ChatMediator;
  private name: string;

  constructor(mediator: ChatMediator, name: string) {
    this.mediator = mediator;
    this.name = name;
  }

  sendMessage(message: string): void {
    console.log(`${this.name} sends: ${message}`);
    this.mediator.sendMessage(this, message);
  }

  receiveMessage(message: string): void {
    console.log(`${this.name} receives: ${message}`);
  }
}

// Usage
const mediator = new ConcreteChatMediator();

const user1 = new User(mediator, "Alice");
const user2 = new User(mediator, "Bob");
const user3 = new User(mediator, "Charlie");

mediator.addUser(user1);
mediator.addUser(user2);
mediator.addUser(user3);

user1.sendMessage("Hello, everyone!");

user2.sendMessage("Hi, Alice!");

user3.sendMessage("Hey, Bob!");
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Memento

> Memento is a behavioral design pattern that lets you save and restore the previous state of an object without revealing the details of its implementation.

<img src="https://github.com/jafari-dev/oop-expert-with-typescript/assets/37804060/4e0b609d-233e-461a-871d-87d5949eb37d"/>

```typescript
class EditorMemento {
  constructor(private readonly content: string) {}

  getContent(): string {
    return this.content;
  }
}

class Editor {
  constructor(private content: string = "") {}

  getContent(): string {
    return this.content;
  }

  setContent(content: string): void {
    this.content = content;
  }

  createSnapshot(): EditorMemento {
    return new EditorMemento(this.content);
  }

  restoreSnapshot(snapshot: EditorMemento): void {
    this.content = snapshot.getContent();
  }
}

class MinimalHistory {
  private snapshots: EditorMemento[] = [];

  push(snapshot: EditorMemento): void {
    this.snapshots.push(snapshot);
  }

  pop(): EditorMemento | undefined {
    return this.snapshots.pop();
  }
}

// Usage
const editor = new Editor();
const minimalHistory = new MinimalHistory();

editor.setContent("Hello, World!");
editor.setContent("Hello, TypeScript!");
minimalHistory.push(editor.createSnapshot());
editor.setContent("Hello, Memento Pattern!");

const lastSnapshot = minimalHistory.pop();

if (lastSnapshot) {
  editor.restoreSnapshot(lastSnapshot);
}

console.log(editor.getContent()); // Output: Hello, TypeScript!
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Observer

> Observer is a behavioral design pattern that lets you define a subscription mechanism to notify multiple objects about any events that happen to the object they’re observing.

<img src="https://github.com/jafari-dev/oop-expert-with-typescript/assets/37804060/6ae11f38-d6ed-4c86-a2ed-b59070a56112"/>

```typescript
interface Subject {
  registerObserver(observer: Observer): void;
  removeObserver(observer: Observer): void;
  notifyObservers(): void;
}

interface Observer {
  update(notification: string): void;
}

class Celebrity implements Subject {
  private followers: Observer[];
  private posts: string[];

  constructor() {
    this.followers = [];
    this.posts = [];
  }

  // Method to make a new post
  sendPost(newPost: string) {
    this.posts = [...this.posts, newPost];
    this.notifyFollowers();
  }

  // Method to notify followers
  private notifyFollowers() {
    this.followers.forEach((follower) => {
      const latestPost = this.posts[this.posts.length - 1];

      follower.update(latestPost);
    });
  }

  // Register a new follower
  registerObserver(observer: Observer) {
    this.followers.push(observer);
  }

  // Remove a follower
  removeObserver(observer: Observer) {
    const index = this.followers.indexOf(observer);
    if (index !== -1) {
      this.followers.splice(index, 1);
    }
  }

  // Notify all followers
  notifyObservers() {
    this.notifyFollowers();
  }
}

class Follower implements Observer {
  private followerName: string;

  constructor(name: string) {
    this.followerName = name;
  }

  // Update method to receive notifications
  update(notification: string) {
    console.log(
      `${this.followerName} received a notification: ${notification}`
    );
  }
}

// Usage
const celebrity1 = new Celebrity();
const celebrity2 = new Celebrity();

const follower1 = new Follower("John");
const follower2 = new Follower("Alice");
const follower3 = new Follower("Bob");

celebrity1.registerObserver(follower1);
celebrity1.registerObserver(follower2);
celebrity2.registerObserver(follower3);

celebrity1.sendPost("Hello World!");
celebrity2.sendPost("I love coding!");

celebrity1.removeObserver(follower1);
celebrity1.removeObserver(follower2);

celebrity1.sendPost("Observer pattern is awesome!");

// Output:
// John received a notification: Hello World!
// Alice received a notification: Hello World!
// Bob received a notification: I love coding!
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### State

> State is a behavioral design pattern that lets an object alter its behavior when its internal state changes. It appears as if the object changed its class.

<img src="https://github.com/jafari-dev/oop-expert-with-typescript/assets/37804060/d77e7972-083f-4fee-b2a7-640f017144f5"/>

```typescript
interface PipelineState {
  start(pipeline: Pipeline): void;
  fail(pipeline: Pipeline): void;
  complete(pipeline: Pipeline): void;
}

class IdleState implements PipelineState {
  start(pipeline: Pipeline) {
    console.log("Pipeline started. Building...");
    pipeline.setState(new BuildingState());
  }

  fail(pipeline: Pipeline) {
    console.log("Pipeline is idle. Nothing to fail.");
  }

  complete(pipeline: Pipeline) {
    console.log("Pipeline is idle. Nothing to complete.");
  }
}

class BuildingState implements PipelineState {
  start(pipeline: Pipeline) {
    console.log("Pipeline is already building.");
  }

  fail(pipeline: Pipeline) {
    console.log("Build failed.");
    pipeline.setState(new FailedState());
  }

  complete(pipeline: Pipeline) {
    console.log("Build complete. Testing...");
    pipeline.setState(new TestingState());
  }
}

class TestingState implements PipelineState {
  start(pipeline: Pipeline) {
    console.log("Pipeline is already in progress.");
  }

  fail(pipeline: Pipeline) {
    console.log("Testing failed.");
    pipeline.setState(new FailedState());
  }

  complete(pipeline: Pipeline) {
    console.log("Testing complete. Deploying...");
    pipeline.setState(new DeployingState());
  }
}

class DeployingState implements PipelineState {
  start(pipeline: Pipeline) {
    console.log("Pipeline is already deploying.");
  }

  fail(pipeline: Pipeline) {
    console.log("Deployment failed.");
    pipeline.setState(new FailedState());
  }

  complete(pipeline: Pipeline) {
    console.log("Deployment successful!");
    pipeline.setState(new IdleState());
  }
}

class FailedState implements PipelineState {
  start(pipeline: Pipeline) {
    console.log("Fix the issues and start the pipeline again.");
  }

  fail(pipeline: Pipeline) {
    console.log("Pipeline already in failed state.");
  }

  complete(pipeline: Pipeline) {
    console.log("Cannot complete. The pipeline has failed.");
  }
}

// 3. Context
class Pipeline {
  private state: PipelineState;

  constructor() {
    // Initial state
    this.state = new IdleState();
  }

  setState(state: PipelineState) {
    this.state = state;
  }

  start() {
    this.state.start(this);
  }

  fail() {
    this.state.fail(this);
  }

  complete() {
    this.state.complete(this);
  }
}

// Client Code
const pipeline = new Pipeline();
pipeline.start(); // Output: Pipeline started. Building...
pipeline.complete(); // Output: Build complete. Testing...
pipeline.fail(); // Output: Testing failed.

pipeline.setState(new BuildingState());
pipeline.start(); // Output: Pipeline is already building.
pipeline.complete(); // Output: Testing complete. Deploying...
pipeline.complete(); // Output: Deployment successful!

pipeline.setState(new TestingState());
pipeline.start(); // Output: Pipeline is already in progress.
pipeline.fail(); // Output: Testing failed.
pipeline.complete(); // Output: Deployment successful!

pipeline.setState(new DeployingState());
pipeline.start(); // Output: Pipeline is already deploying.
pipeline.fail(); // Output: Deployment failed.
pipeline.complete(); // Output: Deployment successful!

pipeline.setState(new FailedState());
pipeline.start(); // Output: Fix the issues and start the pipeline again.
pipeline.fail(); // Output: Pipeline already in failed state.
pipeline.complete(); // Output: Cannot complete. The pipeline has failed.
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Strategy

> Strategy is a behavioral design pattern that lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable.

<img src="https://github.com/jafari-dev/oop-expert-with-typescript/assets/37804060/98b8e8e1-bd42-4205-8237-beff23546128"/>

```typescript
interface RenderStrategy {
  renderShape(shape: Shape): void;
}

class RasterRender implements RenderStrategy {
  renderShape(shape: Shape) {
    console.log(`Raster rendering the ${shape.getName()}`);
  }
}

class VectorRender implements RenderStrategy {
  renderShape(shape: Shape) {
    console.log(`Vector rendering the ${shape.getName()}`);
  }
}

class Shape {
  private name: string;
  private renderStrategy: RenderStrategy;

  constructor(name: string, strategy: RenderStrategy) {
    this.name = name;
    this.renderStrategy = strategy;
  }

  setRenderStrategy(strategy: RenderStrategy) {
    this.renderStrategy = strategy;
  }

  render() {
    this.renderStrategy.renderShape(this);
  }

  getName(): string {
    return this.name;
  }
}

// Usage
const rasterRender = new RasterRender();
const vectorRender = new VectorRender();

const circle = new Shape("Circle", rasterRender);
circle.render();

circle.setRenderStrategy(vectorRender);
circle.render();
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Template Method

> Template Method is a behavioral design pattern that defines the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.

<img src="https://github.com/jafari-dev/oop-expert-with-typescript/assets/37804060/7065675a-ad94-488b-a9b7-96fcb2aa7867"/>

```typescript
abstract class SocialMediaPostAnalyzer {
  private readonly HARMFUL_WORDS = [
    "dumb",
    "stupid",
    "idiot",
    "loser",
    "ugly",
    "fat",
    "skinny",
    "weird",
    "hate",
    "rude",
    "nasty",
  ];

  preprocessData(data: string): string[] {
    return data
      .split(" ")
      .map((word) => word.replace(/[^a-zA-Z ]/g, "").toLowerCase());
  }

  analyze(data: string[]): string[] {
    return data.filter((word) => this.HARMFUL_WORDS.includes(word));
  }

  displayResults(data: string[]): void {
    console.log(
      `The number of harmful words in this post is ${
        data.length
      }, including ${data.join(", ")}.`
    );
  }

  async analyzePosts(): Promise<void> {
    const data = await this.fetchData();
    const preprocessedData = this.preprocessData(data);
    const analyticsResult = this.analyze(preprocessedData);
    this.displayResults(analyticsResult);
  }

  abstract fetchData(): Promise<string>;
}

class TwitterPostAnalyzer extends SocialMediaPostAnalyzer {
  // Fetches data from Twitter API and returns its data
  async fetchData() {
    return ""; // Dummy data
  }
}

class InstagramPostAnalyzer extends SocialMediaPostAnalyzer {
  // Fetches data from Instagram API and returns its data
  async fetchData() {
    return ""; // Dummy data
  }
}

// Usage
const twitterAnalysis = new TwitterPostAnalyzer();
twitterAnalysis.analyzePosts();

const instagramAnalysis = new InstagramPostAnalyzer();
instagramAnalysis.analyzePosts();
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)

### Visitor

> Visitor is a behavioral design pattern that lets you separate algorithms from the objects on which they operate.

<img src="https://github.com/jafari-dev/oop-expert-with-typescript/assets/37804060/592e44d8-f8db-467d-89b1-fea1088820c2"/>

```typescript
interface Visitor {
  visitDesigner(manager: Designer): void;
  visitDeveloper(developer: Developer): void;
}

interface Employee {
  accept(visitor: Visitor): void;
}

class Designer implements Employee {
  name: string;
  numberOfDesignedPages: number;

  constructor(name: string, numberOfDesignedPages: number) {
    this.name = name;
    this.numberOfDesignedPages = numberOfDesignedPages;
  }

  accept(visitor: Visitor): void {
    visitor.visitDesigner(this);
  }
}

class Developer implements Employee {
  name: string;
  baseSalary: number;
  storyPoints: number;

  constructor(name: string, baseSalary: number, storyPoints: number) {
    this.name = name;
    this.baseSalary = baseSalary;
    this.storyPoints = storyPoints;
  }

  accept(visitor: Visitor): void {
    visitor.visitDeveloper(this);
  }
}

class SalaryCalculator implements Visitor {
  totalSalary: number = 0;

  visitDesigner(manager: Designer): void {
    this.totalSalary += manager.numberOfDesignedPages * 200;
  }

  visitDeveloper(developer: Developer): void {
    this.totalSalary += developer.baseSalary + developer.storyPoints * 30;
  }
}

// Usage
const employees: Employee[] = [
  new Designer("Alice", 15),
  new Designer("James", 20),
  new Developer("Ahmad", 3000, 40),
  new Developer("Kate", 2000, 60),
];

const salaryCalculator = new SalaryCalculator();

for (const employee of employees) {
  employee.accept(salaryCalculator);
}

console.log("Total salary:", salaryCalculator.totalSalary);
```

[`⬆ BACK TO TOP ⬆`](#table-of-contents)
